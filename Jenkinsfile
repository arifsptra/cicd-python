node {
    stage('Build') {
        docker.image('python:2-alpine').inside {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
            stash name: 'compiled-results', includes: 'sources/*.py*'
        }
    }

    stage('Test') {
        steps {
            script {
                docker.image('qnib/pytest').inside {
                    sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
                }
                node('linux') {
                    checkout scm
                    try {
                        unstash 'app'
                        sh 'make check'
                    } finally {
                        junit 'test-reports/results.xml'
                    }
                }
            }
        }
    }
}

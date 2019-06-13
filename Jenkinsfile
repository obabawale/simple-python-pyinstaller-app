pipeline {
    agent none 
    stages {
        stage('Build') { 
            agent {
                docker {
                    image 'python:2-alpine' 
                }
            }
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py' 
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'qnib/pytest'
                }
            }
            steps {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }

        stage('Deliver') { 
            agent {
                docker {
                    image 'cdrx/pyinstaller-linux:python2' 
                }
            }
            steps {
                sh 'pyinstaller --onefile sources/add2vals.py'
            }
            post {
                success {
                    sh 'echo "Im deploying here... added ssh key to remote server"'
                    sh 'cd ~ && touch sample.txt'
                    sh 'echo `$whoami` && echo `$pwd`'
                    //sh 'ssh root@128.199.133.72 && cd /opt && chmod a+x add2vals_final'
                    archiveArtifacts 'dist/add2vals' 
                }
            }
        }
    }
}

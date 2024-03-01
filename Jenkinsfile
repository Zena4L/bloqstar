pipeline {
    agent {
        node {
            label 'docker-agent-alpine'
        }
    }
    triggers {
        pollSCM '*/5 * * * *'
    }
    stages {
        stage('Build') {
            steps {
                echo "Building.."
                sh '''
                echo "doing build stuff.."
                '''
            }
        }
        stage('Test') {
            steps {
                echo "Testing.."
                sh '''
                echo "doing test stuff.."
                '''
            }
        }

        stage('Deliver') {
            steps {
                echo 'Deliver....'
                sh '''
                echo "doing delivery stuff.."
                '''
            }
        }
         stage("CleanUp"){
                    when {
                        anyOf {
                            branch 'dev';
                            branch 'staging';
                        }
                    }
                    steps{
                        sh 'docker rmi zena07/blogstar:latest'
                        sh "docker logout"
                        cleanWs()
                        }
                    }
                }
    }

    post {
        success {
            emailext subject: 'CI/CD Pipeline Notification',
                      body: 'Your build was successful! ✨ 🍰 ✨',
                      to: 'zenerbogyah7@gmail.com',
                      attachLog: true
        }
        failure {
            emailext subject: 'CI/CD Pipeline Notification',
                      body: 'Your build failed. Please investigate.❌ ❌ ❌',
                      attachLog: true,
                      to: 'zenerbogyah7@gmail.com'
        }
    }
}

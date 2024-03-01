pipeline {
    agent {
        node {
            label 'docker-agent-alphine'
            }

      }
   triggers { pollSCM '*/5 * * * *' }
   stage('Build With Maven') {
          //build on every branch
          steps {
              sh './mvnw wrapper:wrapper'
          }
      }

       stage('Build Docker Image') {
          // only on specified branches
           when {
                  anyOf {
                      branch 'dev';
                      branch 'staging';
                  }
              }
          steps {
              sh 'docker build -t zena07/blogstar:latest .'
          }
      }

//        stage('Login to Docker Hub') {
//           when {
//                   anyOf {
//                       branch 'dev';
//                       branch 'staging';
//                   }
//               }
//           steps {
//               withCredentials([usernamePassword(credentialsId: 'DOCKER_HUB_CREDENTIALS', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
//                   sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
//               }
//
//           }
//       }

      stage('Push Docker Image to Registry') {
          when {
                  anyOf {
                      branch 'staging';
                      branch 'dev';
                  }
              }
          steps {
              sh 'docker push zena07/blogstar:latest'

          }
      }

//       stage('Deploy') {
//           when {
//                   anyOf {
//                       branch 'dev';
//                       branch 'staging';
//                   }
//               }
//           steps {
//               script{
//                       withCredentials([
//                           file(credentialsId: 'EC2_SSH_KEY', variable: 'EC2_SSH_KEY'),
//                           usernamePassword(credentialsId: 'EC2_CRED', usernameVariable: 'USERNAME', passwordVariable: 'HOST_IP')]) {
//                           sh 'cp $EC2_SSH_KEY ./sshkey'
//                           sh 'chmod 600 sshkey'
//                           sh """
//                               ssh -i "sshkey" $USERNAME@$HOST_IP \
//                               cd /home/ubuntu/gpu-configurator
//                               sudo docker system prune --force
//                               sudo docker pull zena07/blogstar:latest
//                               sudo docker rm -f blogstar
//                               sudo docker run --env-file .env -d  -p 8080:8080 --name gpu-configurator zena07/blogstar:latest
//                           """
//
//                       }
//                   }
//           }
//
//       }

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
              emailext subject: 'CI/CD Pipeline Notification. ',
                        body: 'Your build failed. Please investigate.❌ ❌ ❌ ',
                        attachLog: true,
                        to: 'zenerbogyah7@gmail.com'
          }
}
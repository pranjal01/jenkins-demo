pipeline {
    agent any
    environment{
      DOCKERHUB_CREDENTIALS=credentials('pl-docker-hub')
    }

    stages {
        stage('git') {
            steps {
                echo '-----------------pulling code from source-------------------'
                git branch: 'main', credentialsId: 'sg-git-lab-ssh', url: 'git@gitlab.com:sachin.gangil/jenkins-demo.git'
            }
        }
   
        stage('Build') {
            steps {
                echo '----------------building image of source code---------'
                sh 'docker build -t pranjal01/jenkins-python-docker-demo:v$BUILD_NUMBER .'
               
            }
        }
          stage('Test') {
            steps {
                echo '----------------Listing images--------'
                 sh 'docker images'
            }
          }
   

        stage('Login') {
            steps {
                echo '---------------------Logging in Docker-Hub to push the builded image--------------------'
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('Push') {
            steps {
                echo '--------------------------Image pushed to Docker-Hub successfully----------------'
                sh 'docker push pranjal01/jenkins-python-docker-demo:v$BUILD_NUMBER'
            }
        }
         stage('PullImage') {
            steps {
                 sh 'docker pull pranjal01/jenkins-python-docker-demo:v$BUILD_NUMBER'
                echo '-----------------image pullled successfully------------'
                sh 'docker run -d --name=python-test$BUILD_NUMBER -p 50$BUILD_NUMBER:5000 pranjal01/jenkins-python-docker-demo:v$BUILD_NUMBER'
                echo '-------------container running successfully--------------------'
            }
          }
    }
    post{
      always{
        sh 'docker logout'
      }
    }
}
    }
}

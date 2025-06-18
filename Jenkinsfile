pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'sushmajanaki/hello-app:latest'
    }

    stages {
        stage('Clone') {
          steps {
              git branch: 'main', url: 'https://github.com/SushmaJanaki12/jenkins1.git'
          }
        }


        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh '''
                        echo "$PASSWORD" | docker login -u "$USERNAME" --password-stdin
                        docker push $DOCKER_IMAGE
                    '''
                }
            }
        }

        stage('Deploy Locally') {
            steps {
                sh '''
                    docker stop hello-app || true
                    docker rm hello-app || true
                    docker run -d -p 8080:80 --name hello-app $DOCKER_IMAGE
                '''
            }
        }
    }
}

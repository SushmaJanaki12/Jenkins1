pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds') // Jenkins credentials ID
        DOCKER_IMAGE = 'sushmajanaki12/hello-service'
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/SushmaJanaki12/jenkins1.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: "$DOCKERHUB_CREDENTIALS", url: '']) {
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Deploy Locally') {
            steps {
                sh 'docker run -d -p 5000:5000 $DOCKER_IMAGE'
            }
        }
    }
}

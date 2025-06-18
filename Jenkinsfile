pipeline {
  agent any

  environment {
    IMAGE_NAME = 'yourdockerhub/hello-service'
  }

  stage('Clone') {
    steps {
        git 'https://github.com/SushmaJanaki12/jenkins1.git'
    }
  }


    stage('Build Docker Image') {
      steps {
        sh 'docker build -t $IMAGE_NAME .'
      }
    }

    stage('Push to Docker Hub') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
          sh 'echo $PASS | docker login -u $USER --password-stdin'
          sh 'docker push $IMAGE_NAME'
        }
      }
    }

    stage('Deploy Locally') {
      steps {
        sh 'docker run -d -p 5000:5000 $IMAGE_NAME'
      }
    }
  }
}

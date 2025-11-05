pipeline {
  agent any

  environment {
      DOCKERHUB_USER = "your-dockerhub-username"
  }

  stages {

    stage('Build Docker Images') {
      steps {
        sh """
        docker build -t ${DOCKERHUB_USER}/product-service:latest microservices/product-service
        docker build -t ${DOCKERHUB_USER}/order-service:latest microservices/order-service
        """
      }
    }

    stage('Push to DockerHub') {
      steps {
        withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'PASS')]) {
          sh """
          echo $PASS | docker login -u ${DOCKERHUB_USER} --password-stdin
          docker push ${DOCKERHUB_USER}/product-service:latest
          docker push ${DOCKERHUB_USER}/order-service:latest
          """
        }
      }
    }

    stage('Deploy') {
      steps {
        sh """
        docker compose down || true
        docker compose up -d
        """
      }
    }
  }
}

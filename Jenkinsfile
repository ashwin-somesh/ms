pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/your/repo/microservices.git'
      }
    }

    stage('Build Docker Images') {
      steps {
        sh 'docker build -t your-dockerhub/product-service:latest product-service'
        sh 'docker build -t your-dockerhub/order-service:latest order-service'
      }
    }

    stage('Push Images') {
      steps {
        withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'PASS')]) {
          sh 'echo $PASS | docker login -u yourdockeruser --password-stdin'
        }
        sh 'docker push your-dockerhub/product-service:latest'
        sh 'docker push your-dockerhub/order-service:latest'
      }
    }

    stage('Deploy') {
      steps {
        sh 'docker compose down'
        sh 'docker compose up -d'
      }
    }
  }
}

pipeline {
  agent any

  environment {
    IMAGE_NAME = 'your-dockerhub-username/flask-app'
    IMAGE_TAG = "${env.BUILD_NUMBER}"
    KUBECONFIG_CREDENTIALS_ID = 'kubeconfig'  // Replace with your Jenkins credential ID for K8s
    DOCKER_CREDENTIALS_ID = 'dockerhub'      // Replace with your Jenkins DockerHub credentials ID
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          docker.build("${IMAGE_NAME}:${IMAGE_TAG}", './app')
        }
      }
    }

    stage('Push Docker Image') {
      steps {
        script {
          docker.withRegistry('', "${DOCKER_CREDENTIALS_ID}") {
            docker.image("${IMAGE_NAME}:${IMAGE_TAG}").push()
          }
        }
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        withCredentials([file(credentialsId: "${KUBECONFIG_CREDENTIALS_ID}", variable: 'KUBECONFIG')]) {
          sh '''
          export KUBECONFIG=$KUBECONFIG
          kubectl apply -k k8s/overlays/dev
          '''
        }
      }
    }
  }
}

pipeline {

  environment {
    dockerimagename = "stoyanov808/app"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
          checkout scm
      }
    }

    stage('Build image') {
      steps {
        script {
          dockerImage = docker.build(dockerimagename, '-f backend/Dockerfile backend')
        }
      }
    }

    stage('Pushing Image') {
      environment {
        registryCredential = 'dockerhublogin'
      }
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Verify files') {
  steps {
    sh 'ls -R k8s/base'
  }
}
    stage('Deploy App') {
      steps {
        kubernetesDeploy(configs: "k8s/base/deployment.yaml", kubeconfigId: "kubernetes")
      }
    }

  }

}

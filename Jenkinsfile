pipeline {

  environment {
    dockerimagename = "stoyanov808/app"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git branch: 'main', url: 'https://github.com/Yoana21318/DevOpsFlaskProj.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build(dockerimagename, 'app')
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhublogin'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying App to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "k8s/base/deployment.yaml", kubeconfigId: "kubernetes")
        }
      }
    }

  }

}

pipeline {

  environment {
    dockerimagename = "guneetsingh08/wiley-python-final"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checking Source Files') {
      steps {
        git 'https://github.com/guneet-08/WileyProductionFinal.git'
      }
    }

    stage('Building Image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhub-credentials'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying Python container to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yaml", "service.yaml")
        }
      }
    }
  }
}
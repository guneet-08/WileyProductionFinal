pipeline {

  environment {
    dockerimagename = "guneetsingh08/wiley-python-final"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checking Source Files') {
      steps {
        git branch: 'main', credentialsId: '5abbb4be-a484-4e0b-8d72-e36a20b6aa8b', url: 'https://github.com/guneet-08/WileyProductionFinal.git'
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
          sh 'kubectl apply -f deployment.yaml --context minikube'
          sh 'kubectl apply -f service.yaml --context minikube'
        }
      }
    }
  }
}

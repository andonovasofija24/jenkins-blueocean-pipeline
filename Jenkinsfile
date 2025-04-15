pipeline {
  agent any
  stages {
    stage('Clone repository') {
      steps {
        checkout scm
      }
    }

    stage('Build image') {
      steps {
        script {
          dockerImage = docker.build("${IMAGE_NAME}")
        }

      }
    }

    stage('Push image') {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-creds') {
            dockerImage.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
            dockerImage.push("${env.BRANCH_NAME}-latest")
          }
        }

      }
    }

  }
  environment {
    IMAGE_NAME = 'andonovasofija24/jenkins-blueocean-pipeline'
  }
}
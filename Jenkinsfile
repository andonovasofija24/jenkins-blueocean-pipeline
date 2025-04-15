pipeline {
    agent any

    environment {
        IMAGE_NAME = 'andonovasofija24/jenkins-blueocean-pipeline'
    }

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

        stage('Deploy container') {
            steps {
                script {
                    sh '''
                    docker rm -f jenkins-homework || true
                    docker pull ${IMAGE_NAME}:${BRANCH_NAME}-latest
                    docker run -d --name jenkins-homework -p 3000:3000 ${IMAGE_NAME}:${BRANCH_NAME}-latest
                    '''
                }
            }
        }
    }
}

pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-simple-app"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Clone Code') {
            steps {
                echo 'Cloning source code...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Stop any existing container
                    sh "docker rm -f ${IMAGE_NAME} || true"
                    // Run the app container
                    sh "docker run -d --name ${IMAGE_NAME} -p 5000:5000 ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}

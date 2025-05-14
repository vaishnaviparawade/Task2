pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-simple-app"
        CONTAINER_NAME = "my-simple-app"
        PORT = "5000"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/vaishnaviparawade/Task2.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image..."
                    sh "docker build -t ${IMAGE_NAME}:latest ."
                }
            }
        }

        stage('Stop Existing Container (if any)') {
            steps {
                script {
                    echo "Checking for existing container using port ${PORT}..."
                    sh """
                        CONTAINER_ID=\$(docker ps -q --filter "publish=${PORT}")
                        if [ ! -z "\$CONTAINER_ID" ]; then
                            echo "Stopping container using port ${PORT}..."
                            docker stop \$CONTAINER_ID
                            docker rm \$CONTAINER_ID
                        fi
                    """
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    echo "Running new container..."
                    sh "docker run -d --name ${CONTAINER_NAME} -p ${PORT}:5000 ${IMAGE_NAME}:latest"
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful!"
        }
        failure {
            echo "❌ Deployment failed."
        }
    }
}

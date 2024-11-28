pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = "bhaveshgitstar/flask-hello-world"  // Replace with your Docker Hub repo
        CONTAINER_NAME = "bhaveshgitstar/flask-hello-world"
        DOCKER_USERNAME = "bhasha12"  // Replace with your Jenkins credentials ID for Docker Hub username
        DOCKER_PASSWORD = "Sharma@123"  // Replace with your Jenkins credentials ID for Docker Hub password
    }

    stages {
        // Stage to log in to Docker Hub
        stage('Login to Docker Hub') {
            steps {
                echo 'Logging into Docker Hub...'

                // Use Docker credentials stored in Jenkins
                withCredentials([usernamePassword(credentialsId: 'docker-credentials-id', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    // Log in to Docker Hub using username and password
                    sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                }
            }
        }

        // Build Docker Image for Flask app
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker Image...'
                // Build the Docker image from the current directory (Make sure Dockerfile is present)
                sh 'docker build -t $DOCKER_HUB_REPO:latest .'
            }
        }

        // Push the Docker image to Docker Hub (optional)
        stage('Push Docker Image') {
            steps {
                echo 'Pushing Docker Image to Docker Hub...'
                // Push the image to Docker Hub
                sh 'docker push $DOCKER_HUB_REPO:latest'
            }
        }

        // Run the Docker container (optional for deployment in a test environment)
        stage('Run Docker Container') {
            steps {
                echo 'Running Docker container...'
                // Run the container from the built image (in detached mode)
                sh 'docker run -d --name $CONTAINER_NAME -p 5000:5000 $DOCKER_HUB_REPO:latest'
            }
        }

        // Optional: Clean up the Docker container
        stage('Cleanup') {
            steps {
                echo 'Cleaning up Docker resources...'
                // Stop and remove the container and image (if required)
                sh 'docker stop $CONTAINER_NAME || true'
                sh 'docker rm $CONTAINER_NAME || true'
                sh 'docker rmi $DOCKER_HUB_REPO:latest || true'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Build and deploy completed successfully!'
        }
        failure {
            echo 'Build or deploy failed.'
        }
    }
}

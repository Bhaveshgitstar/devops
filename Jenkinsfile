pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = "bhaveshgitstar/flask-hello-world"
        CONTAINER_NAME = "friendly_ritchie"
        DOCKER_USERNAME = "saumitra"
        DOCKER_PASSWORD = "sid"
    }

    stages {
        // Stage to log in to Docker Hub
        stage('Login to Docker Hub') {
            steps {
                echo 'Logging into Docker Hub...'
                
                // Use Jenkins credentials to securely manage Docker Hub login details
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh "echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USERNAME --password-stdin"
                }
            }
        }

        // Build Stage
        stage('Build') {
            steps {
                echo 'Building Docker Image...'
                // Build the Docker image and tag it with the latest tag
                sh 'docker image build -t $DOCKER_HUB_REPO:latest .'
            }
        }

    }
}

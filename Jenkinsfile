pipeline {
    agent any

    environment {
        // Define your image name
        DOCKER_IMAGE = "my-nginx-app"
    }

    stages {
        stage('Checkout') {
            steps {
                // This pulls the code from your Git repo
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Builds the image using the Dockerfile in the root
                    sh "docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} ."
                    sh "docker tag ${DOCKER_IMAGE}:${BUILD_NUMBER} ${DOCKER_IMAGE}:latest"
                }
            }
        }

        stage('Remove Old Container') {
            steps {
                script {
                    // Stops and removes the container if it's already running to avoid port conflicts
                    sh "docker stop my-nginx-container || true"
                    sh "docker rm my-nginx-container || true"
                }
            }
        }

        stage('Deploy Container') {
            steps {
                // Runs the new container on port 8787
                sh "docker run -d --name my-nginx-container -p 8787:80 ${DOCKER_IMAGE}:latest"
            }
        }
    }

    post {
        success {
            echo "Deployment successful! Access your site at http://your-jenkins-ip:8787"
        }
        failure {
            echo "Deployment failed. Check the logs."
        }
    }
}

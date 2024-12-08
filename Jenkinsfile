pipeline {
    agent any
    
    environment {
        // Set any environment variables you might need
        DOCKER_IMAGE_NAME = 'productservice' // Change this to your desired Docker image name
        DOCKER_TAG = 'latest'        // Tag for the Docker image
        GIT_REPO = 'https://github.com/anilyadav05/ProductService.git' // Replace with your Git repo URL
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                script {
                    // Clone the Git repository
                    git url: "${GIT_REPO}", branch: 'main' // Replace 'main' with your desired branch
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image from the Dockerfile in the repository
                    sh """
                    docker build -t ${DOCKER_IMAGE_NAME}:${DOCKER_TAG} .
                    """
                }
            }
        }
    }
    
    post {
        always {
            // Clean up, e.g., remove any temporary Docker images
            sh "docker system prune -af"
        }
    }
}

pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE_NAME = 'productservice'
        DOCKER_TAG = 'latest'
        GIT_REPO = 'https://github.com/anilyadav05/ProductService.git'
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    // Clone the Git repository
                    git url: "${GIT_REPO}", branch: 'main'
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    sh "docker build -t ${DOCKER_IMAGE_NAME}:${DOCKER_TAG} ."
                }
            }
        }
    }
    
    post {
        always {
            // Clean-up Docker images
            sh "docker system prune -af"
        }
    }
}

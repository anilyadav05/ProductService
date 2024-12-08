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
                git url: "${GIT_REPO}", branch: 'main'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE_NAME}:${DOCKER_TAG} ."
            }
        }
        
        stage('Run Docker Container') {
            steps {
                sh "docker run -d --name ${DOCKER_IMAGE_NAME} ${DOCKER_IMAGE_NAME}:${DOCKER_TAG}"
            }
        }
    }
    
    post {
        always {
            sh "docker system prune -af || true"
        }
    }
}

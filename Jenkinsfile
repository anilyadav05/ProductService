pipeline {
      agent {
        docker {
            image 'docker:19.03.12-dind'  // Use Docker-in-Docker image
            args '--privileged'  // Allows the container to run Docker commands
        }
    }
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
                   success {
            slackSend channel: '#all-productservice', 
                        tokenCredentialId:'slack-bot-token',
                      message: "SUCCESS: Pipeline completed successfully for ${env.JOB_NAME} - Build #${env.BUILD_NUMBER}. :tada:"
        }
        
        failure {
            slackSend channel: '#all-productservice', 
                   tokenCredentialId:'slack-bot-token',
                      message: "FAILURE: Pipeline failed for ${env.JOB_NAME} - Build #${env.BUILD_NUMBER}. :x:"
        }
    }
}

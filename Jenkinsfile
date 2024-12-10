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
                  script{
                  withDockerRegistry(credentialsId: '37b80184-687c-4abb-967b-00f8c2843614', toolName: 'docker') {
                      sh "docker build -t ${DOCKER_IMAGE_NAME}:${DOCKER_TAG} ."
                  }
                  }
                
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
        // Send a Slack notification on success
        slackSend(
            baseUrl: '', // Optional: Leave empty if using Slack plugin defaults
            teamDomain: 'productservice',
            channel: '#all-productservice',
            color: 'good', // Green for success
            tokenCredentialId: 'slack-bot-token',
            botUser: true,
            message: "SUCCESS: Pipeline completed successfully for ${env.JOB_NAME} - Build #${env.BUILD_NUMBER}. :tada:"
        )
    }
    failure {
        // Send a Slack notification on failure
        slackSend(
            baseUrl: '', // Optional: Leave empty if using Slack plugin defaults
            teamDomain: 'productservice',
            channel: '#all-productservice',
            color: 'danger', // Red for failure
            tokenCredentialId: 'slack-bot-token',
            botUser: true,
            message: "FAILURE: Pipeline failed for ${env.JOB_NAME} - Build #${env.BUILD_NUMBER}. :x:"
        )
    }
    }
}

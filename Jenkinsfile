pipeline {
    parameters {
        string(name: 'DOCKER_IMAGE', defaultValue: 'docker:24.0.2', description: 'Docker image to use')
    }
    agent {
        docker {
            image "${params.DOCKER_IMAGE}"
            label 'docker-enabled-node'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    environment {
        DOCKER_IMAGE_NAME = 'productservice'
        DOCKER_TAG = 'latest'
        GIT_REPO = 'https://github.com/anilyadav05/ProductService.git'
        REGISTRY_URL = 'https://hub.docker.com' // Replace with your Docker registry
        REGISTRY_CREDENTIALS = '85af8d9f-a8eb-4b21-84ee-df0b8e81f5a7' // Jenkins credentials ID for registry
        IMAGE_NAME = 'anilyadav05/productservice' // Replace with microservice name
    }
      stages {
        
        stage('Clone Repository') {
            steps {
                git url: "${GIT_REPO}", branch: 'main'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${REGISTRY_URL}/${IMAGE_NAME}:${BUILD_NUMBER} ."
                }
            }
        }
        stage('Login to Docker Registry') {
            steps {
                script {
                    sh "echo $DOCKER_REGISTRY_PASSWORD | docker login -u $DOCKER_REGISTRY_USERNAME --password-stdin ${REGISTRY_URL}"
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    sh "docker push ${REGISTRY_URL}/${IMAGE_NAME}:${BUILD_NUMBER}"
                }
            }
        }
    }
    
post {
    always {
        // Cleanup Docker artifacts
        script {
            try {
                sh "docker system prune -af || true"
            } catch (Exception e) {
                echo "Failed to clean up Docker artifacts: ${e.getMessage()}"
            }
        }
    }
    success {
        // Send a Slack notification on success
        script {
            try {
                slackSend(
                    baseUrl: '', // Optional: Leave empty if using Slack plugin defaults
                    teamDomain: 'productservice',
                    channel: '#all-productservice',
                    color: 'good', // Green for success
                    tokenCredentialId: 'slack-bot-token',
                    botUser: true,
                    message: "SUCCESS: Pipeline completed successfully for ${env.JOB_NAME} - Build #${env.BUILD_NUMBER}. :tada:"
                )
            } catch (Exception e) {
                echo "Failed to send Slack notification: ${e.getMessage()}"
            }
        }
    }
    failure {
        // Send a Slack notification on failure
        script {
            try {
                slackSend(
                    baseUrl: '', // Optional: Leave empty if using Slack plugin defaults
                    teamDomain: 'productservice',
                    channel: '#all-productservice',
                    color: 'danger', // Red for failure
                    tokenCredentialId: 'slack-bot-token',
                    botUser: true,
                    message: "FAILURE: Pipeline failed for ${env.JOB_NAME} - Build #${env.BUILD_NUMBER}. :x:"
                )
            } catch (Exception e) {
                echo "Failed to send Slack notification: ${e.getMessage()}"
            }
        }
    }
}

}

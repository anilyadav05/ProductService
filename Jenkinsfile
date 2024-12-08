pipeline {
        agent {
        docker {
            image 'maven:3-alpine'  // Use a Docker image for the pipeline's agent
            label 'docker'          // Specify label if needed for a specific node
            args '-v /tmp:/tmp'     // Optional arguments to pass to the Docker container
        }
    }
    
    environment {
        DOCKER_IMAGE_NAME = 'productservice'
        DOCKER_TAG = 'latest'
        GIT_REPO = 'https://github.com/anilyadav05/ProductService.git'
    }
      stages {
          stage('Install Docker') {
            steps {
                script {
                    echo "Checking if Docker is installed..."
                    // Check if Docker is installed, and install it if not
                    sh '''
                    if ! command -v docker &> /dev/null; then
                        echo "Docker not found, installing..."
                        sudo apt-get update
                        sudo apt-get install -y docker.io
                        sudo systemctl start docker
                        sudo systemctl enable docker
                    else
                        echo "Docker is already installed."
                    fi
                    '''
                }
            }
        }
        
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

pipeline {
    agent any
    stages {
        stage('Checkout Code from Git') {
            steps {
                git branch: "main", url: "https://github.com/akshayshetty709/mern-devops-project1.git"
            }
        }

        stage('Build Backend Docker Image') {
            steps {
                   sh '''
                        echo "Building backend image..."
                        docker build -t backend-app:latest ./backend
                    '''
                }
        }

        stage('Stop & Remove Old Container') {
            steps {
                sh '''
                    echo "Stopping old container..."
                    docker stop backend-container || true
                    docker rm backend-container || true
                '''
            }
        }

        stage('Run Backend Container') {
            steps {
                sh '''
                    echo "Starting new backend container..."
                    docker run -d \
                        --name backend-container \
                        -p 4000:4000 \
                        --restart unless-stopped \
                        backemd-app:latest
                '''
            }
        }

        stage('Verify Backend Running') {
            steps {
                sh '''
                    echo "Waiting for container to start..."
                    sleep 5
                    
                    echo "Checking container status..."
                    docker ps --filter "name=backend-container"
                '''
            }
        }

     post {
        success {
            echo '🎉 Backend deployed successfully!'
            echo "Backend running on http://localhost:4000"
        }
        failure {
            echo '❌ Backend deployment failed!'
            sh 'docker logs ${BACKEND_CONTAINER} || true'
        }
    }
  }


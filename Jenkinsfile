pipeline {
    agent any
    environment {
        DOCKER_BUILDKIT = 1
        DOCKER_HUB_USERNAME = 'legiahoangthien'  // Thay bằng tên người dùng Docker Hub của bạn
        DOCKER_HUB_PASSWORD = credentials('legiahoangthien-dockerhub') // Đảm bảo bạn đã thêm Docker Hub password vào Jenkins Credentials
    }
    stages {
        // Stage 1: Clone repository
        stage('Clone') {
            steps {
                script {
                    // Clone repository từ GitHub
                    git branch: 'main', url: 'https://github.com/Lghthien/Deploys_Jenkin.git'
                }
            }
        }

        // Stage 2: Build Docker image for server (NestJS)
        stage('Build Server Image') {
            steps {
                script {
                    // Build Docker image cho backend (NestJS)
                    sh 'docker build -t $DOCKER_HUB_USERNAME/nestjs-backend:latest ./server'
                }
            }
        }

        // Stage 3: Build Docker image for client (React)
        stage('Build Client Image') {
            steps {
                script {
                    // Build Docker image cho frontend (React)
                    sh 'docker build -t $DOCKER_HUB_USERNAME/react-frontend:latest ./client'
                }
            }
        }

        // Stage 4: Login to Docker Hub
        stage('Login to Docker Hub') {
            steps {
                script {
                    // Đăng nhập vào Docker Hub bằng Jenkins credentials
                    sh "echo \$DOCKER_HUB_PASSWORD | docker login -u \$DOCKER_HUB_USERNAME --password-stdin"
                }
            }
        }

        // Stage 5: Push Docker images to Docker Hub
        stage('Push Images to Docker Hub') {
            steps {
                script {
                    // Push Docker images lên Docker Hub
                    sh 'docker push $DOCKER_HUB_USERNAME/nestjs-backend:latest'
                    sh 'docker push $DOCKER_HUB_USERNAME/react-frontend:latest'
                }
            }
        }

        // Stage 6: Run Docker Compose to start containers
        stage('Deploy with Docker Compose') {
            steps {
                script {
                    // Sử dụng Docker Compose để khởi động các container
                    sh 'docker-compose -f docker-compose.yml up --build -d'
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment succeeded!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}

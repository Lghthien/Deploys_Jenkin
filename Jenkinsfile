pipeline {
    agent any
    environment {
        DOCKER_BUILDKIT = 1
    }
    stages {
        // Stage 1: Clone repository
        stage('Clone') {
            steps {
                git 'https://github.com/Lghthien/Deploys_Jenkin.git'
            }
        }
        
        // Stage 2: Cài đặt dependencies cho client
        stage('Install Client Dependencies') {
            steps {
                dir('client') {
                    script {
                        sh 'npm install'
                    }
                }
            }
        }

        // Stage 3: Cài đặt dependencies cho server
        stage('Install Server Dependencies') {
            steps {
                dir('server') {
                    script {
                        sh 'npm install'
                    }
                }
            }
        }

        // Stage 4: Xây dựng client
        stage('Build Client') {
            steps {
                dir('client') {
                    script {
                        sh 'npm run build'
                    }
                }
            }
        }

        // Stage 5: Xây dựng server
        stage('Build Server') {
            steps {
                dir('server') {
                    script {
                        sh 'npm run build'
                    }
                }
            }
        }

        // Stage 6: Xây dựng Docker Images
        stage('Build Docker Images') {
            steps {
                script {
                    // Xây dựng Docker image cho client
                    sh 'docker build -t client-image ./client'
                    
                    // Xây dựng Docker image cho server
                    sh 'docker build -t server-image ./server'
                }
            }
        }

        // Stage 7: Triển khai lên Docker
        stage('Deploy to Docker') {
            steps {
                script {
                    // Chạy Docker container cho client
                    sh 'docker run -d --name client-container client-image'
                    
                    // Chạy Docker container cho server
                    sh 'docker run -d --name server-container server-image'
                }
            }
        }
    }

    post {
        success {
            echo 'Triển khai thành công!'
        }
        failure {
            echo 'Triển khai thất bại.'
        }
    }
}

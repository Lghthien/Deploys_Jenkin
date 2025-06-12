pipeline {
    agent any
    environment {
        DOCKER_BUILDKIT = 1
    }
    stages {
        // Stage 1: Clone repository
        stage('Clone') {
            steps {
                script {
                    // Clone repository from GitHub
                    git branch: 'main', url: 'https://github.com/Lghthien/Deploys_Jenkin.git'
                }
            }
        }
        
        // Stage 2: Install Client Dependencies
        stage('Install Client Dependencies') {
            steps {
                dir('client') {
                    script {
                        // Install npm dependencies for the client
                        sh 'npm install'
                    }
                }
            }
        }

        // Stage 3: Install Server Dependencies
        stage('Install Server Dependencies') {
            steps {
                dir('server') {
                    script {
                        // Install npm dependencies for the server
                        sh 'npm install'
                    }
                }
            }
        }

        // Stage 4: Build Client
        stage('Build Client') {
            steps {
                dir('client') {
                    script {
                        // Build the client
                        sh 'npm run build'
                    }
                }
            }
        }

        // Stage 5: Build Server
        stage('Build Server') {
            steps {
                dir('server') {
                    script {
                        // Build the server
                        sh 'npm run build'
                    }
                }
            }
        }

        // Stage 6: Build Docker Images
        stage('Build Docker Images') {
            steps {
                script {
                    // Build Docker image for the client
                    sh 'docker build -t client-image ./client'
                    
                    // Build Docker image for the server
                    sh 'docker build -t server-image ./server'
                }
            }
        }

        // Stage 7: Deploy to Docker
        stage('Deploy to Docker') {
            steps {
                script {
                    // Run Docker container for client
                    sh 'docker run -d --name client-container client-image'
                    
                    // Run Docker container for server
                    sh 'docker run -d --name server-container server-image'
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

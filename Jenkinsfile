pipeline {
    agent any
    environment {
        DOCKER_BUILDKIT = 1
        DOCKER_HUB_USERNAME = 'legiahoangthien' 
        DOCKERHUB_CREDENTIALS = credentials('legiahoangthien-dockerhub')
    }
    stages {
        // Stage 1: Clone repository
        stage('Clone') {
            steps {
                script {
                    git branch: 'main', url: 'https://github.com/Lghthien/Deploys_Jenkin.git'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv(installationName: 'sq1') {
                    sh '''
                    npx sonar-scanner \
                    -Dsonar.projectKey=Deploys_Jenkin \
                    -Dsonar.sources=./client,./server \
                    '''
                }
            }
        }

        stage('Build Server Image') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_HUB_USERNAME/nestjs-backend:latest ./server'
                }
            }
        }

        stage('Build Client Image') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_HUB_USERNAME/react-frontend:latest ./client'
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                }
            }
        }

        stage('Push Images to Docker Hub') {
            steps {
                script {
                    sh 'docker push $DOCKER_HUB_USERNAME/nestjs-backend:latest'
                    sh 'docker push $DOCKER_HUB_USERNAME/react-frontend:latest'
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                script {
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

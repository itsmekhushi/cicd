pipeline {
    agent {
        docker {
            image 'khushi1305meht/getting-started' // Use Docker-in-Docker image
            args '--privileged --name docker'
        }
    }
    environment {
        DOCKER_CREDENTIALS_ID = 'Khushi' // Replace with your Jenkins credentials ID
        IMAGE_NAME = "khushi1305meht/${env.JOB_NAME}:latest" // Replace 'your-username' with Docker Hub username
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: env.DOCKER_CREDENTIALS_ID, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME --target=test .' // Build the test image
            }
        }

        stage('Run Tests') {
            steps {
                sh 'docker run --rm $IMAGE_NAME npm test' // Replace with actual test command
            }
        }

        stage('Build and Push Production Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME --target=prod .' // Build production image
                sh 'docker push $IMAGE_NAME' // Push image to Docker Hub
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

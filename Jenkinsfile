pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE_NAME = 'yourusername/yourimagename:latest'
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh "docker build -t $DOCKER_IMAGE_NAME ."
                }
            }
        }

        stage('Login Docker Hub and Push Docker Image') {
            steps {
                script {
                    // Log in to Docker Hub securely
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                        sh "echo $DOCKER_HUB_PASSWORD | docker login -u $DOCKER_HUB_USERNAME --password-stdin"

                        // Push the Docker image to Docker Hub
                        sh "docker push $DOCKER_IMAGE_NAME"
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker images
            sh 'docker system prune -af'
        }
        success {
            echo 'Pipeline Success'
            // Sending an email notification with details about the success
            mail bcc: '', body: "Docker image build and push successful.", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "Success CI: Project name -> ${env.JOB_NAME}", to: "admin@example.com"
        }
        failure {
            echo 'Pipeline Failed'
            // Sending an email notification with details about the failure
            mail bcc: '', body: "Docker image build and push failed.", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "admin@example.com"
        }
    }
}

pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'my-java-app:latest'
        DOCKER_REGISTRY = 'docker.io'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the Git repository
                git branch: 'main', url: 'https://github.com/vatsav123/my-java-app.git'
            }
        }

        stage('Build') {
            steps {
                // Build the Java application using Maven
                sh 'mvn clean install'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build the Docker image for the application
                script {
                    docker.build(DOCKER_IMAGE)
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                // Push the Docker image to DockerHub (Optional)
                script {
                    docker.withRegistry("https://${DOCKER_REGISTRY}", 'dockerhub-credentials') {
                        docker.image(DOCKER_IMAGE).push()
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                // Deploy the Docker container (This step depends on your infrastructure)
                script {
                    // For example, you might SSH into your server and deploy the Docker container
                    sh "ssh user@server 'docker run -d --name my-java-app ${DOCKER_IMAGE}'"
                }
            }
        }
    }

    post {
        success {
            echo 'Build completed successfully.'
        }
        failure {
            echo 'Build failed.'
        }
    }
}

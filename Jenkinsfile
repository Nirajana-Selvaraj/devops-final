pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "nirajana23/dockerized-html-app"
        DOCKER_CREDENTIALS_ID = "dockerhub-credentials"  // set this in Jenkins
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Make sure to checkout the correct branch (e.g., 'main')
                git branch: 'main', url: 'https://github.com/Nirajana-Selvaraj/devops-final.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        dockerImage.push("latest")
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    sh 'docker rm -f my-web || true'
                    sh "docker run -d --name my-web -p 8080:80 ${DOCKER_IMAGE}:latest"
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Pipeline Failed.'
        }
    }
}

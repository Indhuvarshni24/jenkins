pipeline {
    agent any

    environment {
        IMAGE_NAME = "Indhuvarshni/myapp"
        REGISTRY = "docker.io"
        DOCKER_CREDENTIALS_ID = "docker"
        GITHUB_CREDENTIALS_ID = "github"
        APP_DIR = "/home/vboxuser/jenkins/Jenkinsfile"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git credentialsId: GITHUB_CREDENTIALS_ID, url: 'https://github.com/Indhuvarshni24/jenkins', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $IMAGE_NAME:latest ."
                }
            }
        }

        stage('Login to Docker Registry') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    }
                }
            }
        }

        stage('Push Image to Docker Registry') {
            steps {
                script {
                    sh "docker push $IMAGE_NAME:latest"
                }
            }
        }

        stage('Deploy using Docker Compose') {
            steps {
                script {
                    sh "docker-compose up -d"
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully! '
        }
        failure {
            echo 'Pipeline failed! Check the logs for errors.'
        }
    }
}

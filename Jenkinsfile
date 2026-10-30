pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "YOUR_DOCKERHUB_USERNAME/laravel-app:latest"
        CREDENTIALS_ID = "dockerhub-credentials-id"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/sikumalik987610-a11y/laravel-three-tier-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.withRegistry('', env.CREDENTIALS_ID) {
                        def app = docker.image("${env.DOCKER_IMAGE}")
                        app.build(".")
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', env.CREDENTIALS_ID) {
                        def app = docker.image("${env.DOCKER_IMAGE}")
                        app.push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/'           }
        }
    }
}

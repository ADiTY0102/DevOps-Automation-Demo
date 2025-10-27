pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-login')  // Jenkins credential ID
        DOCKER_IMAGE = "codetoearn/demo-image-pull"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning repository from GitHub...'
                git branch: 'main', url: 'https://github.com/ADiTY0102/DevOps-Automation-Demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t $DOCKER_IMAGE:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                echo 'Pushing image to Docker Hub...'
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh 'docker push $DOCKER_IMAGE:latest'
            }
        }

        stage('Clean Up') {
            steps {
                echo 'Removing local images...'
                sh 'docker rmi $DOCKER_IMAGE:latest || true'
            }
        }
    }

    post {
        success {
            echo '✅ Docker Image Built and Pushed Successfully!'
        }
        failure {
            echo '❌ Pipeline Failed. Check logs for errors.'
        }
    }
}

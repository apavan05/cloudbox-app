pipeline {
    agent any

    environment {
        AWS_REGION = 'eu-north-1'
        ECR_REGISTRY = '658214609078.dkr.ecr.eu-north-1.amazonaws.com'
        ECR_REPOSITORY = 'cloudbox-app'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t cloudbox-app .'
                sh 'docker tag cloudbox-app:latest $ECR_REGISTRY/$ECR_REPOSITORY:latest'
            }
        }

        stage('Login to ECR') {
            steps {
                sh '''
                    aws ecr get-login-password --region $AWS_REGION | \
                    docker login --username AWS --password-stdin $ECR_REGISTRY
                '''
            }
        }

        stage('Push Image to ECR') {
            steps {
                sh 'docker push $ECR_REGISTRY/$ECR_REPOSITORY:latest'
            }
        }
    }
}
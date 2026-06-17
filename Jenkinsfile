pipeline {
    agent any

    environment {
        AWS_REGION = 'eu-north-1'
        AWS_ACCOUNT_ID = '658214609078'
        ECR_REPOSITORY = '658214609078.dkr.ecr.eu-north-1.amazonaws.com/cloudbox-app'
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
                sh 'docker tag cloudbox-app:latest $ECR_REPOSITORY:latest'
            }
        }

        stage('Login to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION | \
                docker login --username AWS --password-stdin $ECR_REPOSITORY
                '''
            }
        }

        stage('Push Image to ECR') {
            steps {
                sh 'docker push $ECR_REPOSITORY:latest'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker pull $ECR_REPOSITORY:latest

                docker stop cloudbox-app || true
                docker rm cloudbox-app || true

                docker run -d \
                  --name cloudbox-app \
                  -p 80:80 \
                  $ECR_REPOSITORY:latest
                '''
            }
        }
    }
}
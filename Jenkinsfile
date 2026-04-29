pipeline {
    agent any

    environment {
        AWS_REGION = "us-east-1"
        ECR_REPO = "970776446242.dkr.ecr.us-east-1.amazonaws.com/hello-app"
    }

    stages {

        stage('ECR Login') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION |
                docker login --username AWS --password-stdin $ECR_REPO
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t hello-app .'
            }
        }

        stage('Tag & Push to ECR') {
            steps {
                sh '''
                docker tag hello-app:latest $ECR_REPO:latest
                docker push $ECR_REPO:latest
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deploy.yaml'
            }
        }
    }
}
pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID = 'AKIA5G2VGFJVHLXEWRMT'
        AWS_SECRET_ACCESS_KEY = '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'
        AWS_DEFAULT_REGION = 'us-east-1'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/JAGATH10/devops-screening.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t jagath10/java_app .
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                sh '''
                docker login -u jagath10 -p Jaga@6385
                docker tag jagath10/java_app:latest jagath10/java_app:latest
                docker push jagath10/java_app:latest
                '''
            }
        }

        stage('Deploy on EC2') {
            steps {
                sh '''
                # Stop any running container before deployment
                docker stop my-node-app || true
                docker rm my-node-app || true
                
                # Pull latest image and deploy
                docker pull jagath10/java_app:latest
                docker run -d -p 9090:8080 --name java_app --restart unless-stopped jagath10/java_app:latest
                '''
            }
        }
    }
}
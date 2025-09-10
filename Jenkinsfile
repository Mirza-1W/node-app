pipeline {
    agent any
    environment {
        DOCKER_HOST = 'npipe:////./pipe/docker_engine'  // Use named pipe for Windows Docker
    }
    tools {
        nodejs "NodeJS-24"
    }
    stages {
        stage('Test AWS') {
            steps {
                withAWS(credentials: 'aws-cred', region: 'us-east-1') {
                    bat 'aws sts get-caller-identity'
                }
            }
        }
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Mirza-1W/node-app.git'
            }
        }
        stage('Build') {
            steps {
                bat 'docker build -t node-app .'
            }
        }
        stage('Test') {
            steps {
                bat 'npm install'
                bat 'npm test'
            }
        }
        stage('Deploy') {
            steps {
                bat 'docker stop node-app || exit 0'
                bat 'docker rm node-app || exit 0'
                bat 'docker run -d -p 3000:3000 --name node-app node-app'
            }
        }
        stage('Push to ECR') {
    steps {
        script {
            docker.withRegistry('https://<your-aws-account-id>.dkr.ecr.<region>.amazonaws.com', 'ecr:us-east-1:aws-credentials') {
                docker.image('node-app').push('latest')
            }
        }
    }
}
stage('Deploy to ECS') {
    steps {
        sh 'aws ecs update-service --cluster my-cluster --service my-service --force-new-deployment'
    }
}

    }
}

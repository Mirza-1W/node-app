pipeline {
    agent any
    environment {
        DOCKER_HOST = 'tcp://localhost:2375'
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
    }
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/Mirza-1W/node-app.git'
      }
    }
    stage('Build') {
      steps {
        sh 'docker build -t node-app .'
      }
    }
    stage('Test') {
      steps {
        sh 'npm test'
      }
    }
    stage('Deploy') {
      steps {
        sh 'docker stop node-app || true'
        sh 'docker rm node-app || true'
        sh 'docker run -d -p 3000:3000 --name node-app node-app'
      }
    }
  }
}

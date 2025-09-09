pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/your-username/node-app.git'
      }
    }
    stage('Build') {
      steps {
        sh 'docker build -t node-app .'
      }
    }
    stage('Test') {
      steps {
        sh 'echo "Testing phase (add your test commands here)"'
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

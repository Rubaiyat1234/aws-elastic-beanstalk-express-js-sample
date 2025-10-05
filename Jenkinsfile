pipeline {
  agent {
    docker {
      image 'node:16'
      args '-u root:root'
    }
  }
  stages {
    stage('Install Dependencies') {
      steps {
        sh 'npm install --save'
      }
    }
    stage('Run Tests') {
      steps {
        sh 'npm test'
      }
    }
    stage('Security Scan') {
      steps {
        sh 'npm install -g snyk'
        sh 'snyk test --severity-threshold=high'
      }
    }
    stage('Build Docker Image') {
      steps {
        script {
          sh 'docker build -t rubai12/node-app:latest .'
        }
      }
    }
    stage('Push to Registry') {
      steps {
        withCredentials([string(credentialsId: 'dockerhub-token', variable: 'DOCKER_TOKEN')]) {
          sh 'echo $DOCKER_TOKEN | docker login -u rubai12 --password-stdin'
          sh 'docker push rubai12/node-app:latest'
        }
      }
    }
  }
}

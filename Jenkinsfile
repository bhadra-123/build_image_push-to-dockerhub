pipeline {
  agent any

  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }

  environment {
    DOCKERHUB_CREDENTIALS = credentials('DOCKERHUB_PAT')
  }

  stages {

    stage('Env') {
      steps {
        sh 'printenv'
        sh 'ls -l /var/run/docker.sock'
      }
    }

    stage('Build') {
      steps {
        sh 'docker build -t bhadra-123/dp-alpine:latest .'
      }
    }
//
//     stage('Login') {
//       steps {
//         sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
//       }
//     }
//
//     stage('Push') {
//       steps {
//         sh 'docker push darinpope/dp-alpine:latest'
//       }
//     }

  }

  post {
    always {
      sh 'docker logout'
    }
  }

}
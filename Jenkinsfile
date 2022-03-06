pipeline {
<<<<<<< HEAD
  agent any
=======
  agent { label 'linux' }
>>>>>>> 13b1d5360bbc3610666b16765e1b896804969ad4

  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }

  environment {
    DOCKERHUB_CREDENTIALS = credentials('DOCKERHUB_PAT')
  }

  stages {

    stage('Build') {
      steps {
        sh 'docker build -t bhadra-123/dp-alpine:latest .'
      }
    }

    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }

    stage('Push') {
      steps {
        sh 'docker push bhadra-123/dp-alpine:latest'
      }
    }

  }

  post {
    always {
      sh 'docker logout'
    }
  }

}
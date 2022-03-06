pipeline {
  agent any

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
        withCredentials([usernamePassword(credentialsId: 'DOCKERHUB_PAT', passwordVariable: 'DOCKERHUB_CREDENTIALS_PSW', usernameVariable: 'DOCKERHUB_CREDENTIALS_USR')]) {
          sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
          //sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
        }
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
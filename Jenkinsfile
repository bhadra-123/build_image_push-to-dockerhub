pipeline { 

    environment { 

        registry = "bhadra1234/docker_image_test" 

        registryCredential = 'DOCKERHUB_PAT' 

        dockerImage = '' 

    }

    agent any 

    stages { 

        stage('Cloning our Git') { 

            steps { 
              
              checkout([$class: 'GitSCM', branches: [[name: 'main']], extensions: [], userRemoteConfigs: [[credentialsId: 'GITHUB_PAT', url: 'https://github.com/bhadra-123/build_image_push-to-dockerhub']]])

            }

        } 

        stage('Building our image') { 

            steps { 

                script { 

                    dockerImage = docker.build registry + ":$BUILD_NUMBER" 

                }

            } 

        }

        stage('Deploy our image') { 

            steps { 

                script { 

                    docker.withRegistry( '', registryCredential ) { 

                        dockerImage.push() 

                    }

                } 

            }

        } 

        stage('Cleaning up') { 

            steps { 

                sh "docker rmi $registry:$BUILD_NUMBER" 

            }

        } 

    }
}

/*
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
        sh 'docker build -t ./Dockerfile'
      }
    }

//     stage('Login') {
//         steps {
//             sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
//         }
//     }

    stage('Login') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'DOCKERHUB_PAT', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          sh 'echo $dockerHubPassword | docker login -u $dockerHubUser --password-stdin'
        }
      }
    }

    stage('Push') {
      steps {
        sh 'docker push bhadra1234/dp-alpine:latest'
      }
    }

  }

  post {
    always {
      sh 'docker logout'
    }
  }

}
*/
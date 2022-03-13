//////////////////////////
// Working Pipeline-1 ///
////////////////////////
/*
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
} */

/*
pipeline {
  agent any
 
 stages {
  stage('Docker Build and Tag') {
           steps {
              
                sh 'docker build -t nginxtest:latest .' 
                sh 'docker tag nginxtest nikhilnidhi/nginxtest:latest'
                sh 'docker tag nginxtest nikhilnidhi/nginxtest:$BUILD_NUMBER'
               
          }
        }
     
  stage('Publish image to Docker Hub') {
          
            steps {
        withDockerRegistry([ credentialsId: "dockerHub", url: "" ]) {
          sh  'docker push nikhilnidhi/nginxtest:latest'
          sh  'docker push nikhilnidhi/nginxtest:$BUILD_NUMBER' 
        }
                  
          }
        }
     
      stage('Run Docker container on Jenkins Agent') {
             
            steps {
                sh "docker run -d -p 4030:80 nikhilnidhi/nginxtest"
 
            }
        }
 stage('Run Docker container on remote hosts') {
             
            steps {
                sh "docker -H ssh://jenkins@172.31.28.25 run -d -p 4001:80 nikhilnidhi/nginxtest"
 
            }
        }
    }
}*/


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
        sh 'docker build -t bhadra1234/dp-alpine:$BUILD_NUMBER'
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
        sh 'docker push bhadra1234/dp-alpine:$BUILD_NUMBER'
      }
    }

  }

  post {
    always {
      sh 'docker logout'
    }
  }

}

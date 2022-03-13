pipeline { 
2
    environment { 
3
        registry = "bhadra1234/docker_image_test" 
4
        registryCredential = 'DOCKERHUB_PAT' 
5
        dockerImage = '' 
6
    }
7
    agent any 
8
    stages { 
9
        stage('Cloning our Git') { 
10
            steps { 
11
                checkout([$class: 'GitSCM', branches: [[name: 'main']], extensions: [], userRemoteConfigs: [[credentialsId: 'GITHUB_PAT', url: 'https://github.com/bhadra-123/build_image_push-to-dockerhub']]])
12
            }
13
        } 
14
        stage('Building our image') { 
15
            steps { 
16
                script { 
17
                    dockerImage = docker.build registry + ":$BUILD_NUMBER" 
18
                }
19
            } 
20
        }
21
        stage('Deploy our image') { 
22
            steps { 
23
                script { 
24
                    docker.withRegistry( '', registryCredential ) { 
25
                        dockerImage.push() 
26
                    }
27
                } 
28
            }
29
        } 
30
        stage('Cleaning up') { 
31
            steps { 
32
                sh "docker rmi $registry:$BUILD_NUMBER" 
33
            }
34
        } 
35
    }
36
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
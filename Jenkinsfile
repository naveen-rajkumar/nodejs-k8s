pipeline {
    environment {
        registryCreds = 'Dockerhub'
        registry = "naveen2094/pipelinedemo"
        dockerImage = ''
        registryURL = 'https://hub.docker.com/repository/docker/naveen2094/pipelinedemo'
    }
    
    agent none
    
    stages {
      stage('git checkout') {
          agent {label 'docker'}
        steps {
           checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'gitcreds', url: 'https://github.com/naveen-rajkumar/nodejs-k8s.git']]])     
        }
    }

      stage('build image') {
          agent {label 'docker'}
        steps {
            script {
              dockerImage = docker.build registry + ":$BUILD_NUMBER"
            }
        }
    }

      stage('push image') {
          agent {label 'docker'}
        steps {
          script {
             withCredentials([string(credentialsId: 'Dockerhubcred', variable: 'Dockerhub')]) {
                 sh 'docker login -u naveen2094 -p ${Dockerhub}'
                  dockerImage.push("$BUILD_NUMBER")
              }
          }
        }
    }

}

}

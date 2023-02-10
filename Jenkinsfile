pipeline{
    environment{
        imagename = "prikale/docker-pipeline"
        registryCredential = "Docker-hub-credentials"
        dockerImage = ''
    }
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-API-Token', url: 'https://github.com/priyanka-1994/Docker-Pipeline.git']]])
            }
        }
        stage('Docker build') {
            steps {
                script {
                    dockerImage = docker.build imagename
                }
            }
        }
         stage('Deploy Image') {
            steps {
                script {
                    docker.withRegistry('',registryCredential) {
                        dockerImage.push("$BUILD_ID")
                        dockerImage.push('latest')
                    }
                }
            }
        }
         stage('remove unused image') {
             steps {
                     sh "docker rmi $imagename:$BUILD_ID"
                     sh "docker rmi $imagename:latest"
                    }
                 }
            }
      }

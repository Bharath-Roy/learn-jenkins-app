pipeline {
    agent any 
     environment {
        DOCKER_HOST = 'unix:///var/run/docker.sock' // Set DOCKER_HOST environment variable
    }


    stages{
            stage("w/o docker"){
                steps {
                    sh "echo without docker"
                }
            }
            stage("w/ docker"){
                agent {
                    docker {
                    image 'node:18-alpine' 
                }
                }
                steps {
                    sh "echo with docker"
                    sh "npm --version"
                }
            }
        }
        
    }

}
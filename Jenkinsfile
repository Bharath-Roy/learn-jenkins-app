pipeline {
   agent any
   environment {
       DOCKER_HOST = 'unix:///var/run/docker.sock' // Set DOCKER_HOST environment variable
   }
  
   stages {
       stage(".py-3.8") {
       agent {
           docker {
               image "python:3.8.10"

           }
       }

       steps{
           sh'''
           python3 --version
           '''
       }


       }

  
       stage(".py-3.9"){
           agent{
               docker {
                   image "python:3.9-alpine"
               }
           }
                 steps{
           sh'''
           python3 --version
           '''
       }
       }
       stage(".py-3.10"){
           agent{
               docker {
                   image "python:3.10"
               }
           }
      steps{
           sh'''
           python3 --version
           '''
       }
       }

   }

   }



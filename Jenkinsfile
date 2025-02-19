
pipeline {
   agent any

   environment {
       DOCKER_HOST = 'unix:///var/run/docker.sock' // Set DOCKER_HOST environment variable
       REACT_APP_VERSION = "1.0.$BUILD_ID"
       AWS_DEFAULT_REGION = 'ap-south-1'
   }

   stages {
              stage('Deploy to AWS') {
           agent {
               docker {
                   image 'amazon/aws-cli'
                   reuseNode true
                   args "-u root --entrypoint=''"
               }
           }
           steps {
               withCredentials([usernamePassword(credentialsId: 'my-aws', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) { // some block
               sh '''
                   aws --version
                   yum install jq -y
                   LATEST_TD_REVISON=$(aws ecs register-task-definition --cli-input-json file://aws/task-definition-prod.json | jq '.taskDefinition.revision')
                   echo $LATEST_TD_REVISON--service Learn-JenkinsApp-Service-prod --task-definition learnJenkinsApp-TaskDefinition-prod:$LAT
                   aws ecs update-service --cluster learn-JenkinsApp-Cluster-prod --service Learn-JenkinsApp-Service-prod --task-definition learnJenkinsApp-TaskDefinition-prod:$LATEST_TD_REVISON
                   aws ecs wait service-stable --cluster learn-JenkinsApp-Cluster-prod --services Learn-JenkinsApp-Service-prod
                   
               '''
               }
           }
       }

       stage('Build') {
           agent {
               docker {
                   image 'node:18-alpine'
                   reuseNode true
               }
           }
           steps {
               sh '''
                   node --version
                   npm --version
                   npm ci
                   npm run build
               '''
           }
       }
   }
}


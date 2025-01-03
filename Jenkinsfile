pipeline {

   agent any
   environment {
    NETLIFY_SITE_ID = 'd6eac577-66f8-48e9-a7b4-40cf80439874'
    NETLIFY_AUTH_TOKEN = credentials('netlify-token-2025')
       DOCKER_HOST = 'unix:///var/run/docker.sock' // Set DOCKER_HOST environment variable
   }

   stages {
      

       stage('Build') {
           agent {
               docker {
                   image 'node:18-alpine'
                   reuseNode true
               }
           }
           steps {
               sh '''
                   echo "small changes"
                   ls -la
                   node --version
                   npm --version
                   npm ci
                   npm run build
                   ls -la
               '''
           }
       }
      

        stage('Tests') {
           parallel {
        stage('Unit tests') {
                   steps {
                       echo 'unit tests ran'
                   }
               }

        stage('E2E') {
                   steps {
                       sh 'echo e2e tests ran'
                   }
               }
               stage('Prod E2E'){
                agent{
                    docker{
                        image 'mcr.microsoft.com/playwright:v1.49.1-noble'
                        reuseNode true
                    }
                }
                environment {
                    CI_ENVIRONMENT_URL ="https://peaceful-daffodil-303af5.netlify.app"
                }
                steps {
                    sh '''
                        npm playwright test --reporter=html
                    '''
                }
               }

           }
       }


       stage('Deploy') {
           agent {
               docker {
                   image 'node:18-alpine'
                   reuseNode true
               }
           }
           steps {
               sh '''
                   npm install netlify-cli
                   node_modules/.bin/netlify --version
                   echo "deploying to production. site ID: $NETLIFY_SITE_ID"
                   node_modules/.bin/netlify status
                   node_modules/.bin/netlify deploy --dir=build --prod

               '''
           }
       }       
      
   }

      post {
        always {
            junit 'jest-results/junit.xml'
        }
    }
}
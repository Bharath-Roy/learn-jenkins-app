pipeline{
    agent any
    environment {
    	DOCKER_HOST = 'unix:///var/run/docker.sock' // Set DOCKER_HOST environment variable
	}

    stages{
        /*
        stage("build"){
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                   ls -la
                   node --version
                   npm --version
                   npm ci
                   npm run build
                   ls -la
                '''
            }
        }
        */

        stage ("Test"){
            parallel {
                stage("Unit test"){
                    agent {
                        docker{
                            image 'node:18-alpine'
                            reuseNode true
                        }
                    }
                    steps{
                        sh '''
                        #test -f build/index.html
                        npm test
                        '''

                    }        }
                stage("E2E"){
                    agent {
                        docker{
                            image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                            reuseNode true
                        }
                    }

                    steps{
                        sh '''
                            npm install serve
                            node_modules/.bin/serve -s build &
                            sleep 10
                            npx playwright test --reporter=html
                        '''

                    }
                }
                    }
        }

    }
    post{
        always {
            juint 'jest-results/junit.xml'
            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'play wright HTML Report', reportTitles: '', useWrapperFileDirectly: true])
        }
    }
 }
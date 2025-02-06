pipeline {
	agent any
	environment {
    	DOCKER_HOST = 'unix:///var/run/docker.sock' // Set DOCKER_HOST environment variable
	}


	stages {
    	stage('w/o docker') {
        	steps {
            	sh '''
           	 
                	echo "without docker"
                	ls -la
                    	touch container-no.txt
             	'''
        	}
    	}
   	 
    	stage('w/ docker') {
        	agent {
            	docker {
                	image 'node:18-alpine'
                	reuseNode true
            	}
        	}
        	steps {
            	sh '''
           	 
            	echo "with docker"
            	ls -la
            	touch container-yes.txt
            	'''
        	}
    	}
	}
}


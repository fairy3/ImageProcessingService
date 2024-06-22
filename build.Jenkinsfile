pipeline {
    agent any
   
    environment {
        POLYBOT_IMAGE_NAME = "polybot"
        NGINX_IMAGE_NAME = "nginx:alpine"
    }

    stages {
        stage('Pull nginx image') {
	   steps {
              script {
                sh 'docker pull "$NGINX_IMAGE_NAME"'
              }
           }
        }

	stage('Build polybot image') {
	   steps {
             script {
		sh '''
                  cd polybot
		  docker build -t ${POLYBOT_IMAGE_NAME}:latest .                  
                '''
             }
           }
        }

        stage('Tag and push images') {
          steps {
             script {
             	withCredentials(
               	[usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASS')]){
	       sh '''
                  docker login -u $DOCKER_USERNAME -p $DOCKER_PASS                      
                  docker tag ${POLYBOT_IMAGE_NAME}:latest ${DOCKER_USERNAME}/${POLYBOT_IMAGE_NAME}:${BUILD_NUMBER}
                  docker push ${DOCKER_USERNAME}/${POLYBOT_IMAGE_NAME}:${BUILD_NUMBER}
                  docker push ${NGINX_IMAGE_NAME}
               '''
              }
	   }
	}
      }

        stage('Trigger Deploy') {
           steps {
               script {
               build job: 'deploy', wait: false, parameters: [
               string(name: 'IMAGE_URL', value: "rimap2610/${POLYBOT_IMAGE_NAME}:${BUILD_NUMBER}")
               ]
           }
	  }
        }
    }
}

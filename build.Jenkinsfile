pipeline {
    agent any
   
    environment {
        IMAGE_NAME = "polybot"
        IMAGE_TAG=$IMAGE_NAME:${BUILD_NUMBER}
        DOCKERHUB_REPOSITORY='rimap2610/polybot'
    }

    stages {
        stage('Build docker image') {
          steps {
             withCredentials(
               [usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASS')]
            ) {
                sh '''
                      cd polybot
                      docker login -u $DOCKER_USERNAME -p $DOCKER_PASS
                      echo "'Docker build:'"
                      docker build -t ${IMAGE_NAME}:latest .
                      docker tag ${IMAGE_NAME}:latest ${IMAGE_TAG}
                      docker push ${DOCKERHUB_REPOSITORY}:${IMAGE_TAG}
                    '''
            }
	      }
        }
        stage('Trigger Deploy') {
           steps {
               build job: 'deploy', wait: false, parameters: [
               string(name: 'IMAGE_URL', value: ${DOCKERHUB_REPOSITORY}:${IMAGE_TAG})
               ]
           }
        }
    }
}

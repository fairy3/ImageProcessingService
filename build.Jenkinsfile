pipeline {
    agent any
   
    environment {
      IMAGE_NAME = "polybot"
      IMAGE_TAG = "latest"
      BUILD_NAME = "1"
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
                      docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                      docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${IMAGE_NAME}:${BUILD_NAME}
                      docker push ${DOCKER_USERNAME}/${IMAGE_NAME}/${IMAGE_NAME}:${BUILD_NAME}
                    '''
            }
	      }
        }
        stage('Trigger Deploy') {
           steps {
               build job: 'deploy', wait: false, parameters: [
               string(name: 'IMAGE_URL', value: "rimap2610/$IMS_NAME")
               ]
           }
        }
    }
}

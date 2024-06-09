pipeline {
    agent any
   
    environment {
      IMAGE_NAME='polybot'
      IMAGE_TAG = "latest"
      DOCKERHUB_REPOSITORY='rimap2610/polybot'
      BUILD_NAME = '${BUILD_NAME}'
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
                      docker tag ${IMAGE_NAME}:latest ${DOCKERHUB_REPOSITORY}:${BUILD_NAME}
                      docker push ${DOCKERHUB_REPOSITORY}:${BUILD_NAME}
                    '''
            }
	      }
        }
        stage('Trigger Deploy') {
           steps {
               build job: 'deploy', wait: false, parameters: [
               string(name: 'IMAGE_URL', value: '${DOCKERHUB_REPOSITORY}:${BUILD_NAME}')
               ]
           }
        }
    }
}

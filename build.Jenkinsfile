pipeline {
    agent any
   
    environment {
        IMAGE_NAME = "polybot"
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
                      docker tag ${IMAGE_NAME}:latest ${DOCKER_USERNAME}/${IMAGE_NAME}:${BUILD_NUMBER}
                      docker push ${DOCKER_USERNAME}/${IMAGE_NAME}:${BUILD_NUMBER}
                    '''
            }
	      }
        }
        stage('Trigger Deploy') {
           steps {
               build job: 'deploy', wait: false, parameters: [
               string(name: 'IMAGE_URL', value: "rimap2610/${IMAGE_NAME}:${BUILD_NUMBER}")
               ]
           }
        }
    }
}

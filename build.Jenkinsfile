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
                      docker build -t my-app:latest .
                      docker tag my-app:latest my-app:1
                      docker push ${DOCKER_USERNAME}/polybot/my-app:1
                    '''
            }
	      }
        }
        stage('Trigger Deploy') {
           steps {
               build job: 'deploy', wait: false, parameters: [
               string(name: 'IMAGE_URL', value: "${DOCKER_USERNAME}/polybot/my-app:1")
               ]
           }
        }
    }
}

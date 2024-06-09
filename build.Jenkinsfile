pipeline {
    agent any
   
    environment {
      IMS_NAME = "polybot:${BUILD_NAME}"
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
                      docker build -t $IMG_NAME .
                      docker tag $IMG_NAME rimap2610/$IMG_NAME
                      docker push rimap2610/$IMG_NAME
                    '''
            }
	      }
        }
        stage('Trigger Deploy') {
           steps {
               build job: 'deploy', wait: false, parameters: [
               string(name: 'IMAGE_URL', value: "rimap2610/$IMG_NAME")
               ]
           }
        }
    }
}

pipeline {
    agent any
   
    environment {
      IMS_NAME = "polybot:${BUILD_NAME}"
    }

    triggers {
      githubPush()
    }
	    
    stages {

        stage('Build docker image') {
          steps { 
           withCredentials(
                 [usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERPASS', passwordVariable: 'USERPASS')]
              ) 
	{
           sh '''
           cd polybot

            docker login -u $DOCKER_USERRNAME -p $DOCKER_PASS
            docker build -t polybot:${BUILD_NUMBER} .
            docker tag $IMG_NAME rimap2610/$IMG_NAME
            docker push rimap2610/polybot:${BUILD_NUMBER} 
            '''
	 }
        }
       }
    }
}

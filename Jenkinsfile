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
 
           sh '''
            cd polybot
            echo 'Hello world'
            '''
	 }
        }
    }
}

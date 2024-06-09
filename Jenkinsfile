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
               echo "'Hello world'"
             '''
	      }
        }
        stage('Trigger Deploy') {
           steps {
               build job: 'BotDeploy', wait: false, parameters: [
               string(name: 'IMAGE_URL', value: "rimap2610/$IMG_NAME")
               ]
           }
        }
    }
}

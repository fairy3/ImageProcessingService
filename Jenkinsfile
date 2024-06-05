pipeline {
    agent any
   
    triggers {
      githubPush()
    }
	    
    stages {

            stage('Build docker image') {

                    sh '''
                        cd polybot

                        # docker login -u ... -p ...
                        docker build -t polybot:${BUILD_NUMBER} .
                        # docker tag ..
                        docker push rimap2610/polybot:${BUILD_NUMBER} 
                       '''
        }
       
    }
}

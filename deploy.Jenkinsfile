pipeline {
    agent any
	   
    parameters {
	#IMAGE_URL
    } 
    stages {
        stage('Deploy') {
          steps {
             sh '''
               echo "deploying"
               # deploying to k8s cluster
	       #	ssh ubuntu@123.123.234.34 "docker stop ...; docker run ..."
             '''
	 }
        }
    }
}

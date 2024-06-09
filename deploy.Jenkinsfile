pipeline {
    agent any
	    
    stages {
        stage('Deploy') {
          steps {
             sh '''
               # deploying to k8s cluster
		ssh ubuntu@123.123.234.34 "docker stop ...; docker run ..."
             '''
	 }
        }
    }
}

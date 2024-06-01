pipeline {
    agent any
   
    triggers {
      githubPush()
    }
	    
    stages {
        stage('Notify') {
            steps {
                sh 'push has happened'
            }
        }
    }
}

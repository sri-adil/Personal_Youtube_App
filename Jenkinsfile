pipeline {
	environment {
		ci = ''
    		app = ''
 	 }
  	agent any
    
 	tools {nodejs "localnode"}
    
  	stages {
        
	    stage('Git') {
	       steps {
		git 'https://github.com/thyanmol/Personal_Youtube_App'
	      	}
    	    }
	
		stage('Install dependencies') {
		steps{
             		sh 'npm install'
		}
	}
		/*stage('Test') {				
			  steps{
				script{
					
                        sh 'npm run test'
			}
			}
			  
			
	}*/
     	
	    stage('Build image') {
		 steps{
		    script{
			app = docker.build("81196/youtube_api")
		    }
		 }
	    }

		
	    stage('Push image') {
		 steps{
		    script{
			docker.withRegistry('https://registry.hub.docker.com', 'sri-docker') {
				app.push("${env.BUILD_NUMBER}")
				app.push("latest")
			}
		 }
		echo "Trying to Push Docker Build to DockerHub"
	       }  
           }

	stage('Deploying image with rundeck') {
		steps{
             		build job: 'personalised_youtube(rundeck)'
		}
	}
  
	}
}
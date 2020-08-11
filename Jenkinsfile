pipeline {
    agent any
    stages {
        stage('Lint HTML') {
            steps {
		sh 'echo "Lint check..."'
                sh 'tidy -q -e *.html'
            }
        }
	    
	stage('Build Docker Image') {
   	    steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]){
		    sh 'echo "Building Docker Image..."'
     	    	    sh 'docker build -t bedoebied/clouddevopscapstone .'
		}
            }
        }
	    
	stage('Push Image To Dockerhub') {
   	    steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]){
		    sh 'echo "Pushing Docker Image..."'
     	    	    sh '''
                        docker login -u $USERNAME -p $PASSWORD
			docker push bedoebied/clouddevopscapstone
                    '''
		}
            }
        }
	 

	stage('Set current kubectl context') {
			steps {
				withAWS(region:'us-east-1', credentials:'ecr_credentials') {
					sh '''
						kubectl config use-context arn:aws:eks:us-east-1:559745402149:cluster/bedoebiedcapstoneproject
					'''
				}
			}
		}	

	stage('Deploy blue container') {
	    steps {
		withAWS(credentials: 'ecr_credentials', region: 'us-east-1') {
		    sh 'echo "Deploy blue container..."'
		    sh 'kubectl apply -f ./blue/blue.yaml'
		}
	    }
	}
	    
	stage('Deploy green container') {
	    steps {
		withAWS(credentials: 'ecr_credentials', region: 'us-east-1') {
		    sh 'echo "Deploy green container..."'
		    sh 'kubectl apply -f ./green/green.yaml'
		}
	    }
	}
	    
	stage('Create blue service') {
	    steps {
		withAWS(credentials: 'ecr_credentials', region: 'us-east-1') {
		    sh 'echo "Create blue service..."'
		    sh 'kubectl apply -f ./blue/blue_service.yaml'
		}
	    }
	}
	    
	stage('Update service to green') {
	    steps {
		withAWS(credentials: 'ecr_credentials', region: 'us-east-1') {
		    sh 'echo "Update service to green..."'
		    sh 'kubectl apply -f ./green/green_service.yaml'
		}
	    }
	}	
   }   
	    
}

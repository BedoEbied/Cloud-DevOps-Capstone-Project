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
     	    	    sh 'sudo docker build -t bedoebied/capstone .'
		}
            }
        }
	    
	stage('Push Image To Dockerhub') {
   	    steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]){
		    sh 'echo "Pushing Docker Image..."'
     	    	    sh '''
                       sudo docker login -u $USERNAME -p $PASSWORD
			sudo docker push bedoebied/capstone
                    '''
		}
            }
        }
	 
	stage('Create k8s cluster') {
	    steps {
		withAWS(credentials: 'ecr_credentials', region: 'us-east-1') {
		    sh 'echo "Create k8s cluster..."'
		    sh '''
			eksctl create cluster \
			--name bedoebiedcapstoneproject \
			--version 1.14 \
			--region us-east-1 \
			--nodegroup-name standard-workers \
			--node-type t2.micro \
			--nodes 2 \
			--nodes-min 1 \
			--nodes-max 3 \
			--managed
		'''
		}
	    }
        }
	    
	stage('Configure kubectl') {
	    steps {
		withAWS(credentials: 'ecr_credentials', region: 'us-east-1') {
		    sh 'echo "Configure kubectl..."'
		    sh 'aws eks --region us-east-1 update-kubeconfig --name bedoebiedcapstoneproject' 
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

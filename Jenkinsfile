pipeline {
    agent any
    environment {
        image = ''
        credential = 'eks-credential'
        reg = 'us-east-2'

    }
    stages{

        stage('Linting the index file') {
            steps {
                sh 'tidy -q -e index.html'
            }
        }

        stage('Building image') {
            steps {
                script {
                    image = docker.build('bedoebied/mycapstone')
                }
            }
        }

        stage('Push image to Registry') {
            steps {
                script {
                    docker.withRegistry( '', 'dockerhub' ) {
                        image.push()
                    }
                }
            }
        }

        // stage('Set Current kubectl Context') {
		// 	steps {
        //         withAWS(region:reg, credentials: credential) {
		// 			sh '''
        //                 export PATH=/home/ubuntu/bin/kubectl/platforms/linux/amd64:$PATH
		// 				kubectl config use-context arn:aws:eks:us-east-2:559745402149:cluster/capstonecluster
						
		// 			'''
		// 		}
		// 	}
		// }


        stage('Deploying blue deployment') {
            steps {
                withAWS(region:reg, credentials: credential) {
                    sh 'kubectl apply -f blue/blue.yaml'
                }
            }
        }

        stage('Deploying blue service') {
            steps {
                withAWS(region:reg, credentials: credential) {
                    sh 'kubectl apply -f blue/blue_service.yaml'
                }
            }
        }

        stage('Deploying green deployment') {
            steps {
                withAWS(region:reg, credentials: credential) {
                    sh 'kubectl apply -f green/green.yaml'
                }
            }
        }

        stage('Going green') {
            steps {
                input 'Ready to redirect traffic to green deployments?'
            }
        }

        stage('Deploying green service') {
            steps {
                withAWS(region:reg, credentials: credential) {
                    sh 'kubectl apply -f green/green_service.yaml'
                }
            }
        }
    }
}

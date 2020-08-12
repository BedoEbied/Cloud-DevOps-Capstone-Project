pipeline {
    agent any
    environment {
        image = ''
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

    stage('Creating Kubernetes Cluster') {
        steps {
            withAWS(region:'us-east-2', credentials:'eks-credential') {
                sh '''
                        eksctl create cluster \
                        --name capstonecluster \
                        --version 1.17   \
                        --nodegroup-name capstonenodes \
                        --node-type t2.micro \
                        --nodes 2 \
                        --nodes-min 1 \
                        --nodes-max 2 \
                        --node-ami auto \
                        --region us-east-2 \
                        --zones us-east-2a \
                        --zones us-east-2b \
                        --zones us-east-2c \
                    '''
            }
        }
    }
}

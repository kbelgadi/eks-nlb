pipeline {
    agent {
        docker { image '897964440075.dkr.ecr.eu-west-1.amazonaws.com/ecr_demo_dev:kubectl-helm-0.1' }
    }
    environment {
        EKS_NAME = "eks_demo_dev"
        AWS_REGION = "eu-west-1"
        KUBECONFIG="/tmp/config"
    } 
    stages {
        stage('context') {
            steps {
                sh '''
                  aws eks --region ${AWS_REGION} update-kubeconfig --name ${EKS_NAME}
                '''
            }
        }
        stage ("deploy nginx ingress controler"){
               steps { 
                sh '''
                  kubectl apply -f mandatory.yml
                '''
               }
        }
        stage ("deploy service NLB"){
               steps { 
                sh '''
                  kubectl apply -f nlb-service.yml
                '''
               }
        }
        stage ("check"){
               steps { 
                sh '''
                  echo "done"
                '''
               }
        }
    }
}
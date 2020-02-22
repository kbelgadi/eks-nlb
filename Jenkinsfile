pipeline {
    agent {
        docker { image '897964440075.dkr.ecr.eu-west-1.amazonaws.com/ecr_demo_dev:kubectl-helm-0.1' }
    }
    environment {
        EKS_NAME = "eks_demo_dev"
        DOCKER_REPO = "897964440075.dkr.ecr.eu-west-1.amazonaws.com/ecr_demo_dev"
        KUBECTL_HELM_IMAGE_NAME = "kubectl-helm"
        KUBECTL_HELM_IMAGE_VERSION = "0.1"
        KUBECTL_HELM_IMAGE = "${DOCKER_REPO}:${KUBECTL_HELM_IMAGE_NAME}-${KUBECTL_HELM_IMAGE_VERSION}"
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
        // stage ("check"){
        //        steps { 
        //         sh '''
        //           count=7; 
        //           while [ $(docker run -v /tmp/.kube:/root/.kube --rm ${KUBECTL_HELM_IMAGE} kubectl get pods -l app=apple-app -o 'jsonpath={..status.conditions[?(@.type=="Ready")].status}') != "True" -o $count -eq 0 ]; do 
        //             echo "waiting for pod"
        //             sleep 1
        //             count=$((count - 1))
        //           done
        //         '''
        //        }
        // }
    }
}
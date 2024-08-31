pipeline {
   agent any
   environment {
      PROJECT = 'WELCOME TO K8S B30 BATCHS - Jenkins Class'
      DESTROY = "FALSE"
   }
   stages {
      stage('Check The Kubernetes Access') {
         when {
            expression {
                 env.DESTROY == 'FALSE'
            }
         }
         steps {
            sh 'aws eks --region us-east-2 update-kubeconfig --name k8sb30-cluster-01'
            sh 'kubectl get pods -A'
            sh 'kubectl get ns'
         }
      }
      stage('Deploy Voting App') {
         when {
            expression {
                 env.DESTROY == 'FALSE'
            }
         }
         steps {
            sh 'ls -al'
            sh 'kubectl apply -f 1.votingapp.yml'
            sh 'kubectl get pods,svc'
         }
      }
      stage('Deploy Ingress for Vote & Result') {
         when {
            expression {
                 env.DESTROY == 'FALSE'
            }
         }
         steps {
            sh 'kubectl apply -f 2.ingress.yml'
            sh 'kubectl apply -f 3.ingress-nk.yml'
            sh 'kubectl get ingress'
         }
      }
      stage('Validating Deployment') {
         when {
            expression {
                 env.DESTROY == 'FALSE'
            }
         }
         steps {
            sh 'kubectl get pods,deployment,svc,ing'
         }
      }
      stage('Perform Kubectl Destroy') {
         when {
            expression {
                 env.DESTROY == 'TRUE'
            }
         }
         steps {
            sh 'kubectl delete -f 1.votingapp.yml || exit 0'
            sh 'kubectl delete -f 2.ingress.yml || exit 0'
            sh 'kubectl delete -f 3.ingress-nk.yml || exit 0'
         }
      }
   }
}

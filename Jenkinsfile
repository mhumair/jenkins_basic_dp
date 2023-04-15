pipeline {
    agent any
    stages {
        stage('remove existing image') {
                steps {
                        sh 'export KUBECONFIG=/home/humair/.kube/config'
                        sh 'kubectl config set-cluster minikube --server=https://192.168.49.2:8443 --insecure-skip-tls-verify=true' 
                        sh 'kubectl config set-context minikube --cluster=minikube --user=minikube'
                        sh 'kubectl config use-context minikube'
                        sh 'rm -rf /var/lib/jenkins/workspace/testk8/minikube_cluster/'
                        sh 'git clone https://github.com/mhumair/minikube_cluster.git'
                        sh 'ls -al minikube_cluster/kube/node.yaml'
                        sh 'kubectl --kubeconfig=./config delete -f minikube_cluster/kube/node.yaml || true'
                }
        }

        stage('build image') {
                steps {
                        sh 'docker build minikube_cluster/node_app/ -t humair/node-web-app'
                }
        }

        stage('Deploy to minikube') {
                steps {
                        sh """
                          kubectl --kubeconfig=./config create -f minikube_cluster/kube/node.yaml
                        """
                }
        }
    }
}


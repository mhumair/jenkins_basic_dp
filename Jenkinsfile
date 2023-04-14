pipeline {
    agent any
    stages { 
	stage('remove existing image') {
		steps {
			git clone https://github.com/mhumair/minikube_cluster.git
			
			sh 'ls -al minikube_cluster/kube/node.yaml'
			sh 'kubectl delete -f minikube_cluster/kube/node.yaml'
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
			  kubectl create -f minikube_cluster/kube/node.yaml 
			  minikube service --url node
                        """
		}
	}
    }
}

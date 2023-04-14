pipeline {
    agent any
    stages { 
	stage('remove existing image') {
		steps {
			sh 'pwd'
                        sh 'cd /home/humair/task/k8s/kube/'
			sh 'ls -al /home/humair/task/k8s/kube/node.yaml'
			sh 'kubectl delete -f /home/humair/task/k8s/kube/node.yaml'
                }
        }

	stage('build image') {
		steps {
			sh 'docker build /home/humair/task/k8s/node_app/ -t humair/node-web-app'
		}
	}

	stage('Deploy to minikube') {
		steps {
			sh """
			  kubectl create -f /home/humair/task/k8s/kube/node.yaml 
			  minikube service --url node
                        """
		}
	}
    }
}

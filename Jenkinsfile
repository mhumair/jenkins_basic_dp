pipeline {
    agent any
    environment {
        docker_login = credentials('docker_login')
    }
    stages {
        stage('remove existing image') {
                steps {
                        sh 'rm -rf /var/lib/jenkins/workspace/testk8/minikube_cluster/'
                        sh 'git clone https://github.com/mhumair/minikube_cluster.git'
                        sh 'kubectl --kubeconfig=./config delete -f minikube_cluster/kube/node.yaml || true'
                }
        }

        stage('build image') {
                steps {
                    
                        sh 'docker info'
                        dir("minikube_cluster/node_app/") {
                            sh """
		            docker build --no-cache --pull . -t mhumair/nisum-task:firsttry --build-arg CACHEBUST=$(date +%s)
                            echo '${docker_login}' | docker login --username mhumair --password-stdin 
                            docker push mhumair/nisum-task:firsttry
                            """
                        }   
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


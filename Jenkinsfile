pipeline {
    agent any 

    stages {
        stage('Build') {
            steps {
                script {
                    // Build and push Docker image
                    bat 'docker build -t w9-dd-app .'
                    bat 'docker tag w9-dd-app:latest wilsonbolledula/w9-dh-app:latest'
                    bat 'docker push wilsonbolledula/w9-dh-app:latest'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    bat '''
                    export MINIKUBE_HOME=$HOME
                    export PATH=$PATH:$MINIKUBE_HOME/.minikube/bin
                    minikube start --driver=docker
                    kubectl config use-context minikube
                    '''
                    bat 'kubectl delete deployment my-kube1-deployment'
                    bat 'kubectl apply -f my-kube1-deployment.yaml'
                    bat 'kubectl apply -f my-kube1-service.yaml'
                    bat 'minikube dashboard'
                    echo 'Deploying application...'
                }
            }
        }
    }
}

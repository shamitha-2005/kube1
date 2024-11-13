pipeline {
    agent any 

    stages {
        stage('Build') {
            steps {
                script {
                    // Build and push Docker image
                    bat 'docker build -t w9-dd-app:latest .'
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
                    // Delete and start Minikube cluster
                    bat 'minikube delete'
                    bat 'minikube start'
                    
                    // Enable the dashboard addon
                    bat 'minikube addons enable dashboard'
                    
                    // Apply Kubernetes resources
                    bat 'kubectl apply -f my-kube1-deployment.yaml'
                    bat 'kubectl apply -f my-kube1-service.yaml'
                    
                    // Expose the Kubernetes Dashboard service
                    bat 'minikube dashboard'
                    
                    echo 'Deploying application...'
                }
            }
        }
    }
}

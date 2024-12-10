
pipeline {
    agent any 

    stages {
        stage('Build') {
            steps {
                script {
                    // Build and push Docker image
                    bat 'docker build -t myrepo/app:latest .'
                    bat 'docker push myrepo/app:latest'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    // Run tests
                    echo 'Running tests...'
                    bat 'npm install'  // Install dependencies
                    bat 'npm test'     // Run tests
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Check if Minikube is running
                    bat 'minikube status'

                    // Start Minikube if not running
                    bat 'minikube start'
                    
                    // Enable the dashboard addon
                    bat 'minikube addons enable dashboard'

                    // Apply Kubernetes resources
                    bat 'kubectl apply -f my-kube1-deployment.yaml'
                    bat 'kubectl apply -f my-kube1-service.yaml'
                    
                    // Optionally wait for pods to be available
                    bat 'kubectl get pods'

                    echo 'Deploying application...'
                }
            }
        }
    }
}


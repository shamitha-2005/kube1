pipeline {
    agent any 

    stages {
        stage('Build') {
            steps {
                script {
                    // Build your Docker image
                    bat 'docker build -t w9-dd-app .'
                    bat 'docker tag w9-dd-app:latest wilsonbolledula/w9-dh-app:latest'
                    bat 'docker push wilsonbolledula/w9-dh-app:latest'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    // Run tests here if you have any
                    echo 'Running tests...'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Start Minikube
                    bat 'minikube start'
                    
                    // Enable the dashboard addon
                    bat 'minikube addons enable dashboard'
                    
                    // Deploy your Kubernetes resources
                    bat 'kubectl apply -f my-kube1-deployment.yaml'
                    bat 'kubectl apply -f my-kube1-service.yaml'
                    
                    // Get the Minikube dashboard URL and print it
                    def dashboardUrl = bat(script: 'minikube dashboard --url', returnStdout: true).trim()
                    echo "Kubernetes Dashboard URL: ${dashboardUrl}"
                }
            }
        }
    }
}

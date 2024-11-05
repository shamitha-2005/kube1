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
                    // Run tests here if you have any
                    echo 'Running tests...'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Start Minikube with proxy settings
                    bat 'minikube start --docker-env HTTP_PROXY=http://actual.proxy.server:8080 --docker-env HTTPS_PROXY=http://actual.proxy.server:8080'
                    
                    // Enable metrics server addon
                    bat 'minikube addons enable metrics-server'
                    
                    // Deploy resources
                    bat 'kubectl apply -f my-kube1-deployment.yaml'
                    bat 'kubectl apply -f my-kube1-service.yaml'
                    
                    // Enable and access the Minikube dashboard
                    bat 'minikube addons enable dashboard'
                    
                    // Retrieve and print the dashboard URL without the invalid timeout flag
                    def dashboardUrl = bat(script: 'minikube dashboard --url', returnStdout: true).trim()
                    echo "Kubernetes Dashboard URL: ${dashboardUrl}"
                }
            }
        }
    }
}

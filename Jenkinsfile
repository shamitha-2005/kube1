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
                    
                    // Enable Minikube dashboard
                    bat 'minikube addons enable dashboard'
                    
                    // Retry fetching the dashboard URL until it's available or reach a maximum number of attempts
                    def maxRetries = 10
                    def delay = 10 // seconds
                    def dashboardUrl = ""
                    for (int i = 0; i < maxRetries; i++) {
                        dashboardUrl = bat(script: 'minikube dashboard --url', returnStdout: true).trim()
                        if (dashboardUrl) {
                            echo "Kubernetes Dashboard URL: ${dashboardUrl}"
                            break
                        }
                        echo "Dashboard not ready yet, retrying in ${delay} seconds..."
                        sleep delay
                    }
                    
                    if (!dashboardUrl) {
                        error("Failed to retrieve the Kubernetes Dashboard URL after ${maxRetries} attempts.")
                    }
                }
            }
        }
    }
}

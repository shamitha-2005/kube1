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
                    // Start Minikube with proxy settings (if required)
                    bat 'minikube start --docker-env HTTP_PROXY=http://actual.proxy.server:8080 --docker-env HTTPS_PROXY=http://actual.proxy.server:8080'
                    
                    // Delete existing deployment to avoid immutable field issues
                    bat 'kubectl delete deployment my-kube1-deployment || true' // Ignore errors if it doesn’t exist
                    
                    // Deploy resources
                    bat 'kubectl apply -f my-kube1-deployment.yaml'
                    bat 'kubectl apply -f my-kube1-service.yaml'
                    
                    // Enable the dashboard and retrieve the URL
                    def dashboardUrl = bat(script: 'minikube dashboard --url', returnStdout: true).trim()
                    echo "Kubernetes Dashboard URL: ${dashboardUrl}"
                    
                    echo 'Deploying application...'
                }
            }
        }
    }
}

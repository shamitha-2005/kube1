pipeline {
    agent any 

    environment {
        DOCKER_CREDENTIALS_ID = 'dockerhub'                   // Docker credentials ID
        DOCKER_IMAGE = 'wilsonbolledula/my-kube1'             // Docker Hub repository and image tag
        HTTP_PROXY = 'http://<actual_ip_or_hostname>:8080'    // Replace with actual HTTP proxy
        HTTPS_PROXY = 'http://<actual_ip_or_hostname>:8080'   // Replace with actual HTTPS proxy
        NO_PROXY = 'localhost,127.0.0.1,192.168.49.2'         // Include Minikube IP here
    }

    stages {
        stage('Debug Proxy Settings') {
            steps {
                script {
                    bat '''
                    echo "HTTP_PROXY: %HTTP_PROXY%"
                    echo "HTTPS_PROXY: %HTTPS_PROXY%"
                    echo "NO_PROXY: %NO_PROXY%"
                    curl -I -x %HTTP_PROXY% http://registry.k8s.io || echo "Failed to connect to registry.k8s.io"
                    '''
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Build Docker image
                    bat "docker build -t ${DOCKER_IMAGE} . 2>&1"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push the Docker image to the repository
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        bat 'docker login -u %DOCKER_USER% -p %DOCKER_PASS%'
                        bat "docker push ${DOCKER_IMAGE} 2>&1"
                    }
                }
            }
        }

        stage('Start Minikube') {
            steps {
                script {
                    // Stop and delete any previous Minikube instance
                    bat '''
                    minikube stop || exit 0
                    minikube delete || exit 0
                    '''

                    // Start Minikube with proxy settings
                    bat '''
                    minikube start --docker-env HTTP_PROXY=%HTTP_PROXY% ^
                    --docker-env HTTPS_PROXY=%HTTPS_PROXY% ^
                    --docker-env NO_PROXY=%NO_PROXY%
                    '''
                }
            }
        }

        stage('Enable Minikube Addons') {
            steps {
                script {
                    // Enable required Minikube addons
                    bat '''
                    minikube addons enable storage-provisioner || exit 0
                    minikube addons enable default-storageclass || exit 0
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Apply the Kubernetes YAML file from your repository
                    bat '''
                    minikube kubectl -- apply -f https://raw.githubusercontent.com/Wilsonbolledula/kube1/main/my-kube1-deployment.yaml
                    '''
                }
            }
        }
    }
}

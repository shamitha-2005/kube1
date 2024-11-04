pipeline {
    agent any 

    environment {
        DOCKER_CREDENTIALS_ID = 'dockerhub'                  // Docker credentials ID
        DOCKER_IMAGE = 'wilsonbolledula/my-kube1'            // Docker Hub repository and image tag
        HTTP_PROXY = 'http://your.proxy.server:port'         // Actual HTTP proxy
    }

    stages {
        stage('Build') {
            steps {
                script {
                    // Build Docker image
                    bat "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push the Docker image to the repository
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        bat 'docker login -u %DOCKER_USER% -p %DOCKER_PASS%'
                        bat "docker push ${DOCKER_IMAGE}"
                    }
                }
            }
        }

        stage('Start Minikube') {
            steps {
                script {
                    // Stop and delete any previous Minikube instance
                    bat '''
                    minikube stop || true
                    minikube delete || true
                    '''

                    // Start Minikube with proxy settings
                    minikube start --docker-env HTTP_PROXY=%HTTP_PROXY% \
                    --docker-env HTTPS_PROXY=%HTTPS_PROXY% \
                    --docker-env NO_PROXY=%NO_PROXY% \
                    --kubernetes-version=v1.30.0
                }
            }
        }

        stage('Enable Minikube Addons') {
            steps {
                script {
                    // Enable required Minikube addons
                    bat '''
                    minikube addons enable storage-provisioner --validate=false || true
                    minikube addons enable default-storageclass --validate=false || true
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Apply the Kubernetes YAML file from your repository
                    bat '''
                    minikube kubectl -- apply -f https://raw.githubusercontent.com/Wilsonbolledula/kube1/main/your-kubernetes-file.yaml
                    '''
                }
            }
        }
    }
}

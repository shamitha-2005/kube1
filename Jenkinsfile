pipeline {
    agent any 

    environment {
        DOCKER_CREDENTIALS_ID = 'dockerhub'               // Docker Hub credentials ID
        DOCKER_IMAGE = 'wilsonbolledula/my-kube1'         // Docker Hub repository and image tag
    }

    stages {
        stage('Build') {
            steps {
                script {
                    bat "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
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
                    // Start Minikube if not running
                    bat 'minikube start'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Apply the Kubernetes YAML file from the checked-out repository
                    bat 'kubectl apply -f kube1.yaml'
                }
            }
        }
    }
}

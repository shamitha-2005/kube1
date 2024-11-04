pipeline {
    agent any 

    environment {
        DOCKER_CREDENTIALS_ID = 'wilsonbolledula/******'  // Replace with the exact credentials ID in Jenkins
        DOCKER_IMAGE = 'wilsonbolledula/my-kube1'         // Your Docker Hub repository and image tag
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
    }
}

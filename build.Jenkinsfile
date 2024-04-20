pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'shwetasng/calculator-app'
        DOCKERFILE_PATH = 'Dockerfile'
        CALCULATOR_DIR = 'Calculator'
    }

    stages {

        stage('Build Docker Image') {
            steps {
                        // Build Docker image using Dockerfile and current directory 
                        sh "docker build -t ${DOCKER_IMAGE_NAME} -f ${DOCKERFILE_PATH} ."
                }
            }
        

        stage('Push Docker Image to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DockerHubCredentials', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
                    script {
                        echo "Pushing Docker image: ${DOCKER_IMAGE_NAME}"
                        
                        sh "docker login -u ${DOCKERHUB_USERNAME} -p ${DOCKERHUB_PASSWORD}"

                        sh "docker push ${DOCKER_IMAGE_NAME}"
                    }
                }
            }
        }
    
}
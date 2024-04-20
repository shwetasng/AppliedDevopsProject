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
                // Build Docker image using Dockerfile and current directory ..
                sh "docker build -t ${DOCKER_IMAGE_NAME} -f ${DOCKERFILE_PATH} ."
            }
        }

        stage('Push Docker Image to DOCKER HUB') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'DockerHubCredentials', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
                        echo "Pushing Docker image: ${DOCKER_IMAGE_NAME}"

                        sh "echo ${DOCKERHUB_PASSWORD} | docker login -u ${DOCKERHUB_USERNAME} --password-stdin"
                        
                        sh "docker push ${DOCKER_IMAGE_NAME}"
                    }
                }
            }
        }

        stage('Trigger Deploy') {
            steps {
                 build job: 'CalOpsDeploy', wait: false, parameters: [
                 string(name: 'calculator-app', value: "https://hub.docker.com/repository/docker/shwetasng/calculator-app/")
        ]
    }
}
    }
}

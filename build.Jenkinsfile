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

        

    //     stage('Check Image Vulnerability'){
    //         steps{
    //             script {
    //                 // Set Snyk token as environment variable
    //                 withCredentials([string(credentialsId: 'SNYK_CREDENTIALS', variable: 'SNYK_TOKEN')]) {
    //                     // Perform Snyk container scan
    //                     sh "snyk container test ${DOCKER_IMAGE_NAME} --api-token=${SNYK_TOKEN}"
    //                 }
    //         }
    //     }
    // }



    stage('Run Snyk Security Test') {
    steps {
        script {
             withCredentials([string(credentialsId: 'SNYK_CREDENTIALS', variable: 'SNYK_TOKEN')]){
            // Run Snyk security test on the Docker image
            def snykCommand = "snyk test --docker ${DOCKER_IMAGE_NAME}"
            def snykOutput = sh(script: snykCommand, returnStdout: true).trim()

            // Print Snyk output to console
            echo "Snyk Security Test Output:"
            echo "${snykOutput}"

            // Check Snyk output for vulnerabilities
            if (snykOutput.contains('found 0 issues')) {
                echo "No vulnerabilities found in the Docker image."
            } else {
                error "Vulnerabilities found in the Docker image. Aborting further steps."
            }
        }
    }
}
    }



    // stage('Test Container for Vulnerabilities') {
    //         steps {
    //             script {
    //                 // Set Snyk token as environment variable
    //                 withCredentials([string(credentialsId: 'SNYK_CREDENTIALS', variable: 'SNYK_TOKEN')]) {
    //                     // Perform Snyk container scan
    //                     def snykCommand = "snyk container test ${DOCKER_IMAGE_NAME} --file=${DOCKERFILE_PATH}"
    //                     def snykOutput = sh(script: snykCommand, returnStdout: true).trim()

    //                     echo "Snyk scan output:"
    //                     echo snykOutput

    //                     // Optionally fail the build based on Snyk scan results
    //                     if (snykOutput.contains('Vulnerabilities found')) {
    //                         error "Security vulnerabilities found in the Docker image!"
    //                     }
    //                 }
    //             }
    //         }
    //     }



//      stage('Scan Docker Image for Vulnerabilities') {
//     steps {
//         // Use withCredentials to read Snyk API token
//         withCredentials([string(credentialsId: 'SNYK_CREDENTIALS', variable: 'SNYK_TOKEN')]) {
//             // Run Snyk CLI to scan the Docker image
//             sh "docker run --rm snyk/snyk-cli test --api-token=${SNYK_TOKEN} ${DOCKER_IMAGE_NAME}"
//         }
//     }
// }


//     stage('Trigger Deploy') {
//             steps {
//                  build job: 'CalOpsDeploy', wait: false, parameters: [
//                  string(name: 'calculator-app', value: "https://hub.docker.com/repository/docker/shwetasng/calculator-app/")
//         ]
//     }
// }
}
}

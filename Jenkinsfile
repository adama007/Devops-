pipeline{
    agent any
    triggers {
        pollSCM('*/5 * * * *') 
    }
    tools{
        nodejs "node"
    }
    environment {
        // Initialize global variables here
        APP_NAME="devopsapp"
        DOCKER_USER = "adamchouk23"
        DOCKER_PASS="dockerhub"
        IMAGE_NAME="${DOCKER_USER}"+"/"+"${APP_NAME}"
        IMAGE_TAG = '1.0'   
    }
    stages{
        stage("Initialization"){
            steps{
                script {
                    // Initialize global variables or perform other setup tasks
                    ENV_VAR_1 = 'value1'
                    ENV_VAR_2 = 'value2'
                }
            }
        }
        stage("Cleanup Workspace"){
            steps{
                cleanWs()
            }
        }
        stage("Checkout from SCM"){
            steps{
                git branch: 'main', credentialsId:'github', url:'https://github.com/adama007/Devops-.git'
            }
        }

        stage("Push to Registry"){
            steps{
                script{
                    docker.withRegistry('',DOCKER_PASS){
                        docker_image=docker.build"${IMAGE_NAME}"
                    }
                    docker.withRegistry('',DOCKER_PASS){
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }
        }
       
     stage('DeployToProduction') {
            steps {
                kubeconfig(caCertificate: 'MIIDBjCCAe6gAwIBAgIBATANBgkqhkiG9w0BAQsFADAVMRMwEQYDVQQDEwptaW5pa3ViZUNBMB4XDTIzMTIyNDIyNTYxOVoXDTMzMTIyMjIyNTYxOVowFTETMBEGA1UEAxMKbWluaWt1YmVDQTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAKt++TNJFNGsI3oe1MsmYqHaPK5lAVeXb9ePhz6eIczX7v7DmdmYp8vkDXHq6VR6Yr0qlw35JEMiYY9nakxT/2PQzEjeJs03VMVbBKW0ephlqqCYJZmYCkSmvHZEP5J2KBsFCN8O2k1FEhbt7/RUUt8dDowR1SAXBoFpcizLEpUZO8twDXVu2Gn/BwUzqdWXPsv/Y8F2D+PcbgAG9eOUgZ1lUJEVt5DX5BWTWM2qurfx+SnBXQi7k63qXA2pvzJr+8eOwk8o+yhdQ9ykRlm3KisPgxC1kW2z7jVIy3GXxm7D+fdGtWuyLE9Ey32zRt9LOxC5va9luQJVl5YgfMNHhgUCAwEAAaNhMF8wDgYDVR0PAQH/BAQDAgKkMB0GA1UdJQQWMBQGCCsGAQUFBwMCBggrBgEFBQcDATAPBgNVHRMBAf8EBTADAQH/MB0GA1UdDgQWBBRdt0Xn7mbf6ObyoEDtjOfPNP9WhTANBgkqhkiG9w0BAQsFAAOCAQEAokwNv+4TwZAmWM6FVzxQO97wAvGXMYsY2/wnjEQ8jQ4JWEjut62TsoR5dlMRiu7kI7g+BG5+7qjTzczljmIKhrF7TLnrhLoaeMFw525cNlYNuhscEVwnJ18dbGZ7hF5aNYkhWUcaZrmPK8gQNs954XcDo0yptXhdNHUbZ7YhNP3WwZE77uwNeiSANW1GGwcpZNy6dbd2W3UQ3qvsNLOb5RMuf19rhEUKQGa9PugCbYoAKoTTrjeK8CZtvL3sZKf3RfzoYQ/LQ5qnhK2JetHNoGw0i6WAzwVKrKs6Jbpu17VdkFRqp9PFaAM3tj8DHsxQelTr7uYRW4x7GAl3mzOa5Q==', credentialsId: 'config', serverUrl: 'https://192.168.49.2:8443') {
                 sh 'kubectl apply -f configmap.yaml'
                 sh 'kubectl apply -f deployment-a.yaml'
                 sh 'kubectl rollout restart deployment todo-api'
}
            }
        }
 stage('Cleanup Artifacts') {
    steps {
        script {
            // Supprimer l'image avec la balise spécifiée
            sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
            
            // Supprimer l'image 'latest'
            sh "docker rmi ${IMAGE_NAME}:latest"
        }
    }
}


     }
     
    
}

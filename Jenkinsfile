pipeline {
    agent any
    environment {
        PATH = "/path/to/kubectl:$PATH"
        // Other environment variables
    }

    stages {
        stage('Build') {
            steps {
                script {
                    // Build and push Docker image to Docker Hub
                    docker.withRegistry('https://registry.hub.docker.com', 'docker') {
                        def imageName = 'omkarmule889/assignment'
                        def imageTag = "${env.BUILD_NUMBER}"
                        docker.build(imageName, "-t ${imageName}:${imageTag} .")
                        docker.image(imageName).push("latest")
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                     withKubeConfig([credentialsId: 'kubeconfig']) {
                        sh "kubectl apply -f deployment.yaml" 
                        sh "kubectl apply -f ingress.yaml" 
                        
                       }
                    }
            }
        }
    }
    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
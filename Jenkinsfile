pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    // Build and push Docker image to Docker Hub
                    docker.withRegistry('https://registry.hub.docker.com', 'docker') {
                        def imageName = 'omkarmule889/assignment'
                        def imageTag = "${env.BUILD_NUMBER}"
                        docker.build(imageName, "-t ${imageName}:${imageTag} .")
                        docker.image(imageName).push("${imageTag}")
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Deploy to Kubernetes
                    sh 'kubectl apply -f kubernetes/deployment.yaml'
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

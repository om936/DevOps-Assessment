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

        stage('Build on kubernetes'){
        steps {
            withKubeConfig([credentialsId: 'kubeconfig']) {
                sh 'pwd'
                sh 'cp -R helm/* .'
                sh 'ls -ltrh'
                sh 'pwd'
                sh '/usr/local/bin/helm upgrade --install nodejs-app nodejs --set image.repository=omkarmule889/assignment: --set image.tag=${BUILD_NUMBER}'
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
pipeline {
    agent any

    stages {
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t gopibrahmaiah/frontend:latest ."
                    }
                }
            }
        }
        stage ("trivy scan") {
            steps {
                sh "trivy image  gopibrahmaiah/frontend:latest"
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push gopibrahmaiah/frontend:latest"
                    }
                }
            }
        }
    }
}

pipeline {
    agent any

    stages {
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t gopibrahmaiah/currencyservice:latest ."
                    }
                }
            }
        }
         stage("trivy scan") {
            steps {
                sh "trivy image gopibrahmaiah/currencyservice:latest"
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push gopibrahmaiah/currencyservice:latest "
                    }
                }
            }
        }
    }
}

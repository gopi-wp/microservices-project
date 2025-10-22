pipeline {
    agent any

    stages {
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    dir('src') {

                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t gopibrahmaiah/cartservice:latest ."
                     } 
                   }
                }
            }
        }
        stage("trivy scan") {
            steps {
                sh "trivy image gopibrahmaiah/cartservice:latest"
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push gopibrahmaiah/cartservice:latest "
                    }
                }
            }
        }
    }
}

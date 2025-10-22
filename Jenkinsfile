pipeline {
    agent any
    environment {
        SCANNER_HOME=tool "sonar"
    }
    stages {
        stage ("Soanr") {
            steps {
                withSonarQubeEnv("sonar") {
                        sh """
                            ${SCANNER_HOME}/bin/sonar-scanner \
                            -Dsonar.projectKey=payment\
                            -Dsonar.sources=. \
                            """
                }
            }
        }
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t gopibrahmaiah/paymentservice:latest ."
                    }
                }
            }
        }
        stage ("trivy scan") {
            steps {
                sh "trivy image gopibrahmaiah/paymentservice:latest"
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push gopibrahmaiah/paymentservice:latest "
                    }
                }
            }
        }
    }
}

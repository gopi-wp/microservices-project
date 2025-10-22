pipeline {
    agent any
    environment {
       SCANNER_HOME= "sonar" 

    stages {
        stage ("Sonar") {
            steps {
                withSonarQubeEnv("sonar") {
                        sh """
                            ${SCANNER_HOME}/bin/sonar-scanner \
                            -Dsonar.projectKey=checkoutservice\
                            -Dsonar.sources=. \
                            """
                }
            }
        }
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t gopibrahmaiah/checkoutservice:latest ."
                    }
                }
            }
        }
         stage("trivy scan") {
            steps {
                sh "trivy image gopibrahmaiah/checkoutservice:latest"
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push gopibrahmaiah/checkoutservice:latest "
                    }
                }
            }
        }
    }
}

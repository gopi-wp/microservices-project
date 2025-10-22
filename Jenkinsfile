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
                            -Dsonar.projectKey=emailservice\
                            -Dsonar.sources=. \
                            """
                }
            }
        }
        stage ("trivy scan") {
            steps {
                sh "trivy image gopibrahmaiah/emailservice:latest"
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push gopibrahmaiah/emailservice:latest "
                    }
                }
            }
        }
    }
}

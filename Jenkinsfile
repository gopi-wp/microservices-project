pipeline {
    agent any
    environment {
        SCANNER_HOME=tool "sonar"
    }
    stages {
        stage ("Sonar") {
            steps {
                withSonarQubeEnv("sonar") {
                        sh """
                            ${SCANNER_HOME}/bin/sonar-scanner \
                            -Dsonar.projectKey= adservice\
                            -Dsonar.sources=. \
                            """
                }
            }
        }
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t gopibrahmaiah/adservice:latest ."
                    }
                }
            }
        }

        stage('Trivy Scan') {
            steps {
                sh "trivy image gopibrahmaiah/adservice:latest"
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push gopibrahmaiah/adservice:latest"
                    }
                }
            }
        }
    }
}


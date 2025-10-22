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
                            -Dsonar.projectKey=loadgenerator\
                            -Dsonar.sources=. \
                            """
                }
            }
        }
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t gopibrahmaiah/loadgenerator:latest ."
                    }
                }
            }
        }
        stage ("trivy scan") {
            steps {
                sh "trivy image gopibrahmaiah/loadgenerator:latest"
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push gopibrahmaiah/loadgenerator:latest"
                    }
                }
            }
        }
    }
}

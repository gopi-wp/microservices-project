pipeline {
    agent any
    environment {
        SCANNER_HOME=tool "sonar"
    }
    stage ("CQA") {
            steps {
                withSonarQubeEnv("sonar") {
                      sh "${SCANNER_HOME}/bin/sonar-scanner -Dsonar.projectKey=checkoutservice"
                }
            }
        }
        stage ("QualityGates") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-cred'
                }
            }
        }

    stages {
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t gopibrahmaiah/checkoutservice:latest ."
                    }
                }
            }
        }
        stage ("Scan") {
            steps {
                sh "trivy image gopibrahmaiah/checkoutservice:latest >>appimage.txt"
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

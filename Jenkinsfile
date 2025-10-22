pipeline {
    agent any
    environment {
        SCANNER_HOME=tool "sonar"
    }
    stages {
       stage ("CQA") {
            steps {
                withSonarQubeEnv("sonar") {
                      sh "${SCANNER_HOME}/bin/sonar-scanner -Dsonar.projectKey=shippingservice"
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
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t gopibrahmaiah/shippingservice:latest ."
                    }
                }
            }
        }
        stage ("Scan") {
            steps {
                sh "trivy image gopibrahmaiah/shippingservice:latest >>appimage.txt"
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push gopibrahmaiah/shippingservice:latest "
                    }
                }
            }
        }
    }
}

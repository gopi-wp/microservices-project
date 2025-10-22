pipeline {
    agent any

    environment {
        SCANNER_HOME = tool "sonar"
    }

    stages {
        stage("CQA") {
            steps {
                withSonarQubeEnv("sonar") {
                    sh """
                        ${SCANNER_HOME}/bin/sonar-scanner \
                        -Dsonar.projectKey=cartservice
                    """
                }
            }
        }

        stage("Quality Gates") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-cred'
                }
            }
        }

        stage("Build & Tag Docker Image") {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t gopibrahmaiah/cartservice:latest ."
                    }
                }
            }
        }

        stage("Scan Image with Trivy") {
            steps {
                sh "trivy image gopibrahmaiah/cartservice:latest > appimage.txt"
            }
        }

        stage("Push Docker Image") {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push gopibrahmaiah/cartservice:latest"
                    }
                }
            }
        }
    }
}

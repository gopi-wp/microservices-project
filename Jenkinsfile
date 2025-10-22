pipeline {
    agent any
    environment {
        SCANNER_HOME = tool "sonar" 
    }
    stages {
        stage ("checkout") {
            steps {
                checkout scm
                echo "Building branch: ${env.BRANCH_NAME}"
            }
        }
        stage ("Sonar") {
            steps {
                withSonarQubeEnv("sonar") {
                        sh """
                            ${SCANNER_HOME}/bin/sonar-scanner \
                            -Dsonar.projectKey=${projectKey} \
                            -Dsonar.sources=. \
                            -Dsonar.branch.name=${env.BRANCH_NAME}
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


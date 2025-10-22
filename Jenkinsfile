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
                            -Dsonar.projectKey=cartservice\
                            -Dsonar.sources=. \
                            -Dsonar.branch.name=cartservice
                            """
                }
            }
        }
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

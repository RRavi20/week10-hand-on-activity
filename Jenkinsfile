pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t week10-production-app:2.0 .'
            }
        }

        stage('Security Scan') {
            steps {
                sh 'trivy image --exit-code 1 --severity HIGH,CRITICAL week10-production-app:2.0'
            }
        }

        stage('Test') {
            steps {
                sh 'docker image inspect week10-production-app:2.0'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker rm -f week10-production-app || true
                    docker run -d \
                      --name week10-production-app \
                      -p 8082:80 \
                      week10-production-app:2.0
                '''
            }
        }

        stage('Verify') {
            steps {
                sh 'curl -f http://localhost:8082'
            }
        }
    }
}

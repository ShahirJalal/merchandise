pipeline {
    agent {
        label 'ubuntu'
    }

    environment {
        IMAGE_NAME = 'merchandise-frontend'
        CONTAINER_NAME = 'merchandise-frontend'
        PORT = '8084'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('frontend') {
                    sh 'docker build -t ${IMAGE_NAME}:latest .'
                }
            }
        }

        stage('Stop Existing Container') {
            steps {
                sh '''
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    docker run -d \
                      --name ${CONTAINER_NAME} \
                      -p ${PORT}:80 \
                      --restart unless-stopped \
                      ${IMAGE_NAME}:latest
                '''
            }
        }

    }

    post {

        success {
            echo 'Merchandise deployed successfully!'
        }

        failure {
            echo 'Merchandise deployment failed.'
        }

        always {
            sh 'docker image prune -f || true'
        }
    }
}
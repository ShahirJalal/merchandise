pipeline {
    agent { label 'ubuntu-agent' }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/ShahirJalal/merchandise.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('frontend') {
                    sh '''
                        docker build \
                        -t merchandise-frontend:latest .
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker rm -f merchandise-frontend || true

                    docker run -d \
                        --name merchandise-frontend \
                        -p 8083:80 \
                        --restart unless-stopped \
                        merchandise-frontend:latest
                '''
            }
        }
    }
}
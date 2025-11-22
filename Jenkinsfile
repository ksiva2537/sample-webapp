pipeline {
    agent any

    stages {

        stage('Checkout from GitHub') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/ksiva2537/sample-webapp.git'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t sample-webapp:latest .
                '''
            }
        }

        stage('Save Docker Image') {
            steps {
                sh '''
                    docker save sample-webapp:latest -o sample-webapp.tar
                '''
            }
        }

        stage('Copy Docker Image to Remote Node') {
            steps {
                sh '''
                    scp -o StrictHostKeyChecking=no \
                    -i /var/lib/jenkins/.ssh/id_ed25519 \
                    sample-webapp.tar \
                    deploy@192.168.1.82:/tmp/sample-webapp.tar
                '''
            }
        }

        stage('Deploy Docker Container on Node') {
            steps {
                sh '''
                    ssh -o StrictHostKeyChecking=no \
                    -i /var/lib/jenkins/.ssh/id_ed25519 \
                    deploy@192.168.1.82 \
                    "docker load -i /tmp/sample-webapp.tar && \
                     docker stop sample-webapp || true && \
                     docker rm sample-webapp || true && \
                     docker run -d --name sample-webapp -p 8081:8080 sample-webapp:latest"
                '''
            }
        }
    }
}

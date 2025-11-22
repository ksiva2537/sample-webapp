pipeline {
    agent any

    environment {
        SSH_KEY = "/var/lib/jenkins/.ssh/id_ed25519"
        NODE_USER = "deploy"
        NODE_IP = "192.168.1.82"
        WAR_NAME = "sample-webapp-1.0-SNAPSHOT.war"
        TOMCAT_DIR = "/var/lib/tomcat10/webapps"
    }

    stages {

        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Copy WAR to Node Server') {
            steps {
                sh """
                    scp -o StrictHostKeyChecking=no -i ${SSH_KEY} target/${WAR_NAME} \
                    ${NODE_USER}@${NODE_IP}:/tmp/app.war
                """
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh """
                    ssh -o StrictHostKeyChecking=no -i ${SSH_KEY} ${NODE_USER}@${NODE_IP} \
                    'sudo mv /tmp/app.war ${TOMCAT_DIR}/ROOT.war'
                """
            }
        }
    }

    post {
        success {
            echo "Deployment completed successfully!"
        }
        failure {
            echo "Deployment failed!"
        }
    }
}

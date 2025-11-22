pipeline {
    agent any

    stages {
        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Copy WAR to Node Server') {
            steps {
                sh '''
                scp -i /var/lib/jenkins/.ssh/id_ed25519 target/sample-webapp-1.0-SNAPSHOT.war \
                deploy@192.168.1.82:/tmp/app.war
                '''
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh '''
                ssh -i /var/lib/jenkins/.ssh/id_ed25519 deploy@192.168.1.82 \
                "sudo mv /tmp/app.war /var/lib/tomcat10/webapps/ROOT.war"
                '''
            }
        }
    }
}

pipeline {
    agent any

    stages {
        stage('checkout from git') {
            steps {
                checkout scmGit(
                    branches: [[name: '*/main']],
                    extensions: [],
                    userRemoteConfigs: [[url: 'https://github.com/ksiva2537/sample-webapp.git']]
                )
            }
        }

        stage('building the package') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('deploying the project') {
            steps {
                // Copy WAR file to remote server
                sh 'scp -i /var/lib/jenkins/.ssh/id_ed25519 /var/lib/jenkins/workspace/job5_sample_web/target/sample-webapp-1.0-SNAPSHOT.war deploy@192.168.1.82:/tmp/sample-webapp-1.0-SNAPSHOT.war'
                
                // Correct SSH command with proper quoting
                sh '''
                ssh -i /var/lib/jenkins/.ssh/id_ed25519 deploy@192.168.1.82 \
                "sudo mv /tmp/sample-webapp-1.0-SNAPSHOT.war /var/lib/tomcat10/webapps/sample-webapp-1.0-SNAPSHOT.war"
                '''
            }
        }
    }
}

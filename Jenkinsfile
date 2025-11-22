pipeline {
  agent any
  environment {
    NODE_IP = "13.126.114.136"          // replace
    DEPLOY_USER = "deploy"
    SSH_KEY = "/var/lib/jenkins/.ssh/id_ed25519"  // Jenkins user key path
    TOMCAT_WEBAPP_DIR = "/var/lib/tomcat10/webapps"
  }
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Build') {
      steps {
        sh "mvn -B -DskipTests clean package"
      }
    }
    stage('Deploy to Node') {
      steps {
        // Copy WAR to node as ROOT.war (replace name as desired)
        sh """
          scp -o StrictHostKeyChecking=no -i ${SSH_KEY} target/*.war ${DEPLOY_USER}@${NODE_IP}:${TOMCAT_WEBAPP_DIR}/ROOT.war
          ssh -o StrictHostKeyChecking=no -i ${SSH_KEY} ${DEPLOY_USER}@${NODE_IP} 'sudo systemctl restart tomcat10'
        """
      }
    }
  }
  post {
    success {
      echo "Deployed successfully to ${NODE_IP}"
    }
    failure {
      echo "Build or deploy failed"
    }
  }
}

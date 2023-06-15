pipeline {
  agent none
  
  stages {
    stage('SCM') {
      agent {
        label 'abhi2'
      }
      steps {
        checkout scm
      }
    }
    
    stage('SonarQube Analysis') {
      agent {
        label 'Master'
      }
      steps {
        script {
          def scannerHome = tool 'sonarqube'
          withSonarQubeEnv() {
            sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=webapp -Dsonar.sources=."
          }
        }
      }
    }
  stage('Deploy') {
    agent {
        label 'abhi2'
      }
          steps {
            
            sh 'apt-get update'
            sh 'apt-get install -y nodejs'
            sh 'npm install'

            sh 'npm install -g pm2 || npm install -g pm2@latest' // Install PM2 globally, try latest version if not found

            sh 'pm2 stop all' // Stop any previously running PM2 processes
            sh 'pm2 delete all' // Delete any previously configured PM2 processes

            sh 'scp -r ./src user@192.168.1.100:/path/to/deploy' // Use SCP to deploy the code to the remote server

            sh 'ssh user@192.168.1.100 pm2 start /path/to/deploy/src/index.js --name my-app' // Start the Node.js application with PM2 on the remote server
          
          }
        }
      }

}  


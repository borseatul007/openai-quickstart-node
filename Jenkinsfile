pipeline {
  agent none
  
  stages {
    stage('SCM') {
      agent {
        label 'rnd-server'
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
            sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=demo -Dsonar.sources=."
          }
        }
      }
    }
  stage('Deploy') {
    agent {
        label 'rnd-server'
      }
          steps {
            
            sh 'sudo apt-get update'
            sh 'sudo apt-get install -y nodejs'
            sh 'sudo apt install npm -y'

            sh 'sudo npm install -g pm2 || npm install -g pm2@latest' // Install PM2 globally, try latest version if not found

            //sh 'sudo pm2 stop all' // Stop any previously running PM2 processes
            //sh 'sudo pm2 delete all' // Delete any previously configured PM2 processes
            
            //sh 'sudo scp -r worksapce/nodejs/pages/ ubuntu@15.207.109.102:/var/www/html' // Use SCP to deploy the code to the remote server
            sh 'sudo cp -R /root/jenkins/workspace/nodejs/pages /var/www/html/' 

            //sh 'ssh ubuntu@15.207.109.102 pm2 start /var/www/html/pages/index.js --name my-app' // Start the Node.js application with PM2 on the remote server
            sh 'pm2 start /var/www/html/pages/index.js --name my-app'
          }
        }
      }

}  


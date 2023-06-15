 node('abhi2') {
  stage('SCM') {
    checkout scm
  }

  stage('Deploy') {
    // Code deployment steps go here
    // ...
    def nodejsInstalled = sh(returnStdout: true, script: 'which node').trim()
    if (nodejsInstalled) {
      echo 'Node.js is already installed'
    } else {
      echo 'Installing Node.js'
      sh 'apt-get update'
      sh 'apt-get install -y nodejs'
    }

    def pm2Installed = sh(returnStdout: true, script: 'which pm2').trim()
    if (pm2Installed) {
      echo 'PM2 is already installed'
    } else {
      echo 'Installing PM2'
      sh 'npm install -g pm2 || npm install -g pm2@latest'
    }

    sh 'npm install'

    sh 'pm2 stop all' // Stop any previously running PM2 processes
    sh 'pm2 delete all' // Delete any previously configured PM2 processes

    sh 'scp -r ./pages ubuntu@15.207.109.102:/var/www/html' // Use SCP to deploy the code to the remote server

    sh 'ssh ubuntu@15.207.109.102 pm2 start var/www/html/pages/index.js --name my-app' // Start the Node.js application with PM2 on the remote server
  }
}

node('master') {
  stage('SonarQube Analysis') {
    withSonarQubeEnv('SonarQube') {
      def scannerHome = tool 'sonarqube'
      sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=webapp -Dsonar.sources=."
    }
  }
}

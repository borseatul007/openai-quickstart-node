 pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout your source code repository
                git 'https://github.com/borseatul007/openai-quickstart-node.git'
            }
        }
        
        stage('SCM') {
            checkout scm
            }
        stage('SonarQube Analysis') {
            def scannerHome = tool 'sonarqube';
            withSonarQubeEnv() {
            sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=webapp -Dsonar.sources=. "
         }
        }
    }
}

 pipeline {
    agent any
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

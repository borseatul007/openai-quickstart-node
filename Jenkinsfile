 pipeline {
    agent any

    stages {
        stage('SCM') {
            steps {
                checkout scm
            }
        }
        
        stage('SonarQube Analysis') {
            environment {
                scannerHome = tool name: 'sonarqube', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
            }
            steps {
                script {
                    withSonarQubeEnv('SonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=webapp -Dsonar.sources=."
                    }
                }
            }
        }
    }
}



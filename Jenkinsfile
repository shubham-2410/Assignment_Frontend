pipeline {
    agent any
    tools {
        nodejs 'NodeJS' // Correct NodeJS installation name
        // sonar 'Sonarscanner' // Correct tool type for SonarQube
    }
    environment {
        SONARQUBE_TOKEN = credentials('sonarqube-token') // SonarQube token stored in Jenkins credentials
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                bat 'npm install' // For Windows, replace with 'sh' for Unix-based systems
            }
        }
        stage('Building') {
            steps {
                bat 'npm run build' // For Windows, replace with 'sh' for Unix-based systems
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube-server') { // Match the SonarQube server name configured in Jenkins
                    bat '''
                    sonar-scanner ^
                        -Dsonar.projectKey=RegisterFrontend ^
                        -Dsonar.sources=src ^
                        -Dsonar.exclusions=**/node_modules/**,**/build/** ^
                        -Dsonar.host.url=http://localhost:9000 ^
                        -Dsonar.token=%SONARQUBE_TOKEN%
                    '''
                }
            }
        }
    }
    post {
        success {
            echo "Pipeline Successful"
        }
        failure {
            echo "Pipeline Failed"
        }
    }
}

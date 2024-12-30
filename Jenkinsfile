pipeline {
    agent any
    tools {
        nodejs 'NodeJS'
        sonarQubeScanner 'Sonarscanner' 
    }
    environment {
        SONARQUBE_TOKEN = credentials('sonarqube-token')
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                    bat '''
                    sonar-scanner \
                        -Dsonar.projectKey=RegisterFrontend \
                        -Dsonar.sources=src \
                        -Dsonar.exclusions=**/node_modules/**,**/build/** \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.token=%SONARQUBE_TOKEN%
                    '''
            }
        }
    }
  post{
    success{
      echo "Pipeline Successfull"
    }
    failure{
      echo "Pipeline Failure"
    }
  }
}

pipeline {
    agent any

    environment {
        NODEJS_PATH = 'C:/Program Files/nodejs'
        SONAR_SCANNER_PATH = 'C:/Users/ADMIN/Downloads/sonar-scanner-cli-6.2.1.4610-windows-x64/sonar-scanner-6.2.1.4610-windows-x64/bin'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from SCM
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install NPM dependencies
                bat '''
                set PATH=%NODEJS_PATH%;%PATH%
                npm install
                '''
            }
        }

        stage('Run Tests') {
            steps {
                // Execute frontend tests
                bat '''
                set PATH=%NODEJS_PATH%;%PATH%
                npm test -- --passWithNoTests
                '''
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('Sonarqube-token') // SonarQube token stored in Jenkins credentials
            }
            steps {
                // Run SonarQube analysis
                bat '''
                set PATH=%SONAR_SCANNER_PATH%;%PATH%
                sonar-scanner -Dsonar.projectKey=frontend ^
                    -Dsonar.sources=. ^
                    -Dsonar.host.url=http://localhost:9000 ^
                    -Dsonar.token=%SONAR_TOKEN%
                '''
            }
        }

        stage('Build Application') {
            steps {
                // Build the frontend application
                bat '''
                set PATH=%NODEJS_PATH%;%PATH%
                npm run build
                '''
            }
        }
    }

    post {
        success {
            echo 'Frontend pipeline completed successfully.'
        }
        failure {
            echo 'Frontend pipeline failed. Please check the logs.'
        }
        always {
            echo 'This always runs, regardless of success or failure.'
        }
    }
}

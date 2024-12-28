pipeline {
    agent any

    environment {
        NODEJS_PATH = 'C:/Program Files/nodejs'
        SONAR_SCANNER_PATH = 'C:/Users/ADMIN/Downloads/sonar-scanner-cli-6.2.1.4610-windows-x64/sonar-scanner-6.2.1.4610-windows-x64/bin'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from SCM
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                // Navigate to the frontend directory and install dependencies
                dir('frontend') {
                    bat '''
                    set PATH=%NODEJS_PATH%;%PATH%
                    npm install
                    '''
                }
            }
        }

        stage('Run Tests') {
            steps {
                // Navigate to the frontend directory and run tests
                dir('frontend') {
                    bat '''
                    set PATH=%NODEJS_PATH%;%PATH%
                    npm test -- --passWithNoTests
                    '''
                }
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('Sonarqube-token') // Access the SonarQube token stored in Jenkins credentials
            }
            steps {
                // Perform SonarQube analysis on the frontend code
                dir('frontend') {
                    bat '''
                    set PATH=%SONAR_SCANNER_PATH%;%PATH%
                    sonar-scanner -Dsonar.projectKey=web-form ^
                        -Dsonar.sources=. ^
                        -Dsonar.host.url=http://localhost:9000 ^
                        -Dsonar.token=%SONAR_TOKEN%
                    '''
                }
            }
        }

        stage('Build Application') {
            steps {
                // Navigate to the frontend directory and build the application
                dir('frontend') {
                    bat '''
                    set PATH=%NODEJS_PATH%;%PATH%
                    npm run build
                    '''
                }
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

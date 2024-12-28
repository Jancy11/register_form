pipeline {
    agent any
    tools {
        nodejs 'nodejs-20.11'
    }
    environment {
        SONAR_TOKEN = credentials('sonarqube-token')
    }
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                dir('frontend') {
                    sh 'npm install'
                }
            }
        }
        stage('Run Tests') {
            steps {
                dir('frontend') {
                    sh 'npm test -- --passWithNoTests'
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                dir('frontend') {
                    withSonarQubeEnv('Sonarqube') {
                        sh '''
                        npm install sonar-scanner
                        npx sonar-scanner                         -Dsonar.projectKey=frontend-web                         -Dsonar.sources=src                         -Dsonar.host.url=http://<sonarqube-server>:9000                         -Dsonar.login=$SONAR_TOKEN
                        '''
                    }
                }
            }
        }
        stage('Build Application') {
            steps {
                dir('frontend') {
                    sh 'npm run build'
                }
            }
        }
    }
    post {
        success {
            echo 'Pipeline executed successfully.'
        }
        failure {
            echo 'Pipeline failed. Please check logs.'
        }
    }
}
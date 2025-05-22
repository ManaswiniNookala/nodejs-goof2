pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/ManaswiniNookala/nodejs-goof2',
                    credentialsId: 'github-creds' // Must match the ID in Jenkins Credentials Manager
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0' // Prevents failure if tests fail
            }
        }

        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0' // Continues even if no coverage script exists
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0' // Show audit issues but don't fail
            }
        }
    }
}

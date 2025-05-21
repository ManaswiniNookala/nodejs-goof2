pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Replace 'github-creds' with your actual Jenkins credential ID
                git branch: 'main',
                    credentialsId: 'github-creds',
                    url: 'https://github.com/ManaswiniNookala/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0' // Continue even if tests fail
            }
        }

        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0' // Continue even if script doesn't exist
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0' // Show vulnerabilities without failing the build
            }
        }
    }
}

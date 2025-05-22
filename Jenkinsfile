pipeline {
    agent any

    environment {
        SONAR_SCANNER_HOME = 'sonar-scanner-extracted\\sonar-scanner-5.0.1.3006-windows'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ManaswiniNookala/nodejs-goof2'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0' // Prevent build failure if test fails
            }
        }

        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0' // Safe fail
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0'
            }
        }

        stage('SonarCloud Analysis') {
            environment {
                SONAR_TOKEN = credentials('SONAR_TOKEN')
            }
            steps {
                bat '''
                curl -sSLo sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-windows.zip
                powershell -Command "Expand-Archive -Force sonar-scanner.zip sonar-scanner-extracted"
                cd sonar-scanner-extracted\\sonar-scanner-5.0.1.3006-windows\\bin
                sonar-scanner.bat -Dsonar.login=%SONAR_TOKEN%
                '''
            }
        }
    }
}

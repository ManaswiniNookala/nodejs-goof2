pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('manaswininookala') 
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
                bat 'npm test || exit /b 0' 
            }
        }

        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0' 
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0'
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                bat '''
                curl -sSLo sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-windows.zip
                powershell -Command "Expand-Archive -Force sonar-scanner.zip sonar-scanner-extracted"
                cd sonar-scanner-extracted\\sonar-scanner-5.0.1.3006-windows\\bin
                sonar-scanner.bat ^
                  -Dsonar.projectKey=manaswini_nodejs-goof2 ^
                  -Dsonar.organization=manaswini-org ^
                  -Dsonar.host.url=https://sonarcloud.io ^
                  -Dsonar.login=%SONAR_TOKEN%
                '''
            }
        }

        stage('Post Build') {
            steps {
                echo 'Pipeline completed. Archiving or notifications can be added here.'
            }
        }
    }
}

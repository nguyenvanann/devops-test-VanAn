pipeline {
    agent any

    environment {
        TELEGRAM_TOKEN = credentials('telegram-bot-token')
        TELEGRAM_CHAT_ID = credentials('telegram-chat-id')
    }

    stages {

        stage('Checkout') {
            steps {
                echo '=== CHECKOUT SOURCE ==='
                checkout scm
            }
        }

        stage('Install dependencies') {
            steps {
                echo '=== INSTALL DEPENDENCIES ==='
                bat 'npm install'
            }
        }

        stage('Build') {
            steps {
                echo '=== BUILD PROJECT ==='
                bat 'npm run build'
            }
        }

        stage('Deploy') {
            steps {

                echo '=== DEPLOY STARTED ==='

                bat '''
                    powershell -Command "$message = '🚀 DEPLOY STARTED`n`nProject: devops-test`nBranch: main'; Invoke-RestMethod -Uri ('https://api.telegram.org/bot' + $env:TELEGRAM_TOKEN + '/sendMessage') -Method Post -Body @{chat_id=$env:TELEGRAM_CHAT_ID; text=$message}"
                '''

                echo '=== COPYING WEBSITE ==='

                bat '''
                    if not exist D:\\DevOps-Deploy mkdir D:\\DevOps-Deploy

                    xcopy /E /I /Y /Q dist\\* D:\\DevOps-Deploy\\
                '''

                echo '=== DEPLOY SUCCESS ==='

                bat '''
                    powershell -Command "$message = '✅ DEPLOY SUCCESS`n`nProject: devops-test`nBranch: main`nURL: http://localhost:8081'; Invoke-RestMethod -Uri ('https://api.telegram.org/bot' + $env:TELEGRAM_TOKEN + '/sendMessage') -Method Post -Body @{chat_id=$env:TELEGRAM_CHAT_ID; text=$message}"
                '''
            }
        }
    }

    post {

        failure {
            echo '=== DEPLOY FAILED ==='

            bat '''
                powershell -Command "$message = '❌ DEPLOY FAILED`n`nProject: devops-test`nBranch: main'; Invoke-RestMethod -Uri ('https://api.telegram.org/bot' + $env:TELEGRAM_TOKEN + '/sendMessage') -Method Post -Body @{chat_id=$env:TELEGRAM_CHAT_ID; text=$message}"
            '''
        }

    }
}
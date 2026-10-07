pipeline {
    agent any

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
                echo '=== DEPLOY WEBSITE ==='

                bat '''
                    if not exist D:\\DevOps-Deploy mkdir D:\\DevOps-Deploy

                    xcopy /E /I /Y /Q dist\\* D:\\DevOps-Deploy\\
                '''

                echo '=== DEPLOY SUCCESS ==='
                echo 'Website: http://localhost:8081'
            }
        }
    }

    post {
        success {
            echo '======================================'
            echo 'BUILD SUCCESS'
            echo 'Website: http://localhost:8081'
            echo '======================================'
        }

        failure {
            echo '======================================'
            echo 'BUILD FAILED'
            echo '======================================'
        }
    }
}
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ayushisoni1004/8.2CDevSecOps.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                bat 'npm test > test_log.txt 2>&1 || exit /b 0'
            }
            post {
                always {
                    emailext(
                        subject: "Test Stage: ${currentBuild.currentResult} - Build #${env.BUILD_NUMBER}",
                        body: "The Run Tests stage completed with status: ${currentBuild.currentResult}. See attached log.",
                        to: 'ayushisoni1004@gmail.com',
                        attachmentsPattern: 'test_log.txt'
                    )
                }
            }
        }
        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0'
            }
        }
        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit > audit_log.txt 2>&1 || exit /b 0'
            }
            post {
                always {
                    emailext(
                        subject: "Security Scan: ${currentBuild.currentResult} - Build #${env.BUILD_NUMBER}",
                        body: "The NPM Audit stage completed with status: ${currentBuild.currentResult}. See attached log.",
                        to: 'ayushisoni1004@gmail.com',
                        attachmentsPattern: 'audit_log.txt'
                    )
                }
            }
        }
    }
}

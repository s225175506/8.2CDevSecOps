pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:/opt/homebrew/bin:${env.PATH}"
        NOTIFY_EMAIL = 's225175506@deakin.edu.au'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Tool: Git'
                git branch: 'main', url: 'https://github.com/s225175506/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Tool: npm'
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Tool: npm test (Mocha / the project test runner)'
                sh 'npm test || true'
            }
            post {
                always {
                    emailext(
                        to: "${NOTIFY_EMAIL}",
                        subject: "Run Tests: ${currentBuild.currentResult} - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: """The Run Tests stage has finished.

Job: ${env.JOB_NAME}
Build: #${env.BUILD_NUMBER}
Stage status: ${currentBuild.currentResult}
Console: ${env.BUILD_URL}console

The build log is attached.
""",
                        attachLog: true,
                        compressLog: true
                    )
                }
            }
        }

        stage('Generate Coverage Report') {
            steps {
                echo 'Tool: nyc / Istanbul (npm run coverage)'
                sh 'npm run coverage || true'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                echo 'Tool: npm audit'
                sh 'npm audit || true'
            }
            post {
                always {
                    emailext(
                        to: "${NOTIFY_EMAIL}",
                        subject: "Security Scan: ${currentBuild.currentResult} - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: """The NPM Audit (Security Scan) stage has finished.

Job: ${env.JOB_NAME}
Build: #${env.BUILD_NUMBER}
Stage status: ${currentBuild.currentResult}
Console: ${env.BUILD_URL}console

The build log is attached.
""",
                        attachLog: true,
                        compressLog: true
                    )
                }
            }
        }
    }
}

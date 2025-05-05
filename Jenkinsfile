pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: ' https://github.com/kottochii/8.2CDevSecOps.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm test > npm-test.log 2>&1 || true' // Allows pipeline to continue despite test failures
            }

            post {
                success {
                    emailext (
                        to: "s222271192@deakin.edu.au",
                        subject: "SUCCESS: Job '${env.JOB_NAME}'",
                        body: "Testing succeeded! Log attached.",
                        attachmentsPattern: "**/npm-test.log",
                        debugMode: true  // ← Enables verbose logging
                    )
                }
                failure {
                    emailext (
                        to: "s222271192@deakin.edu.au",
                        subject: "FAILED: Job '${env.JOB_NAME}'",
                        body: "Testing failed. Check logs: ${env.BUILD_URL}console",
                        debugMode: true  // ← Enables verbose logging
                    )
                }
            }
        }
        stage('Generate Coverage Report') {
            steps {
                // Ensure coverage report exists
                sh 'npm run coverage || true'
            }
        }
        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit || true' // This will show known CVEs in the output
            }
        }
    }
}
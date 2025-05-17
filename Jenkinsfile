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
                sh 'npm test > npm-test.log 2>&1' // Allows pipeline to continue despite test failures
            }

            post {#
                success {
                    emailext (
                        to: "s222271192@deakin.edu.au",
                        subject: "SUCCESS: Testing in Job '${env.JOB_NAME}'",
                        body: "Testing succeeded! Log attached.",
                        attachmentsPattern: "**/npm-test.log"
                    )
                }
                failure {
                    emailext (
                        to: "s222271192@deakin.edu.au",
                        subject: "FAILED: Testing in Job '${env.JOB_NAME}'",
                        body: "Testing failed! Log attached.",
                        attachmentsPattern: "**/npm-test.log"
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
                sh 'npm audit  > npm-test.log 2>&1' // This will show known CVEs in the output
            }

            post {
                success {
                    emailext (
                        to: "s222271192@deakin.edu.au",
                        subject: "SUCCESS: Security scan in Job '${env.JOB_NAME}'",
                        body: "Security scanning succeeded! Log attached.",
                        attachmentsPattern: "**/npm-test.log"
                    )
                }
                failure {
                    emailext (
                        to: "s222271192@deakin.edu.au",
                        subject: "FAILED: Security scan in Job '${env.JOB_NAME}'",
                        body: "Security scanning failed! Log attached.",
                        attachmentsPattern: "**/npm-test.log"
                    )
                }
            }
        }
    }
}

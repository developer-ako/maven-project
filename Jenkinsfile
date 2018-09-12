pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
                }
            }
        }
        stage('Deploy to Staging') {
            steps {
                build 'deploy-to-staging'
            }
        }
        stage('Deploy to Production') {
            steps {
                timeout(time:5, unit:'DAYS') {
                    input message: 'Approve PRODUCTION Deployment?', submitter: 'admin'
                }
                build 'deploy-to-prod'
            }
            post {
                success {
                    echo 'Code deployed to Production.'
                }
                failure {
                    echo 'Deployment failed.'
                }
            }
        }
     }
}
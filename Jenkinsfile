pipeline {
    agent any
    parameters {
        string(name: 'tomcat_dev', defaultValue: '13.250.123.248', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultValue: '13.229.247.57', description: 'Production Server')
    }
    triggers {
        pollSCM('* * * * *')
    }
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
        stage('Deployments') {
            parallel {
                stage('Deploy to Staging') {
                    steps {
                        bat "winscp scp:ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps /privatekey=D:\\PIWS\\OneDrive\\DEV\\AWS\\tomcat-demo.ppk /upload D:\\PIWS\\OneDrive\\DEV\\Jenkins\\workspace\\fully-automated-pipeline\\webapp\\target\\*.war"
                    }
                }
                stage('Deploy to Production') {
                    steps {
                        bat "winscp scp:ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps /privatekey=D:\\PIWS\\OneDrive\\DEV\\AWS\\tomcat-demo.ppk /upload D:\\PIWS\\OneDrive\\DEV\\Jenkins\\workspace\\fully-automated-pipeline\\webapp\\target\\*.war"
                    }
                }
            }
        }
     }
}
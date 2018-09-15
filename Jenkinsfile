pipeline {
    agent any
    parameters {
        string(name: 'tomcat_dev', defaultValue: '52.221.189.28', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultValue: '13.250.109.141', description: 'Production Server')
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
                        bat "winscp -i <AWS pem file on your laptop> **target/*.war <user>@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }
                stage('Deploy to Production') {
                    steps {
                        bat "winscp -i <AWS pem file on your laptop> **target/*.war <user>@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
     }
}
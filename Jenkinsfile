pipeline {
    agent any
    environment {
        SSH_CRED = credentials('server-key')
        def CONNECT = 'ssh -o StrictHostKeyChecking=no ubuntu@3.99.221.25'
    }
    stages {
        
        stage('Build') {
            steps {
                echo 'building app'
                sh "pwd"
                sh "ls"
                sh "zip -r webapp.zip ."
                sh "ls"
            }
        }
        stage('artifact') {
            steps {
                echo 'pushing artifact to nexus'
                sh "curl -u admin:password --upload-file webapp.zip http://15.222.8.55:8081/repository/webapp/webapp.zip"
                
            }
        }
       stage('Deploy') {
            steps {
                echo 'Deploying app'
                sshagent(['server-key']) {
                    sh '$CONNECT "curl -u admin:password -O http://15.222.8.55:8081/repository/webapp/webapp.zip"'
                    sh '$CONNECT "sudo apt install zip -y"'
                    sh '$CONNECT "sudo rm -rf /var/www/html/"'
                    sh '$CONNECT "sudo mkdir /var/www/html/"'
                    sh '$CONNECT "sudo unzip webapp.zip -d /var/www/html/"'                    
                }
            }
        }
        stage('Clean-Up') {
            steps {
                echo 'Remove existing files'
                deleteDir()
            }
        }
    }
}
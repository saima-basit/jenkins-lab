pipeline {
    agent any

    environment {
        GITHUB_REPO = 'https://github.com/saima-basit/jenkins-lab.git'
        S3_BUCKET = 'saimasbucket'
        SERVER_IP = 'ec2-3-104-117-100.ap-southeast-2.compute.amazonaws.com'
        SERVER_USERNAME = 'ubuntu'
        APP_NAME = 'website.html'
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    deleteDir()
                    git branch: "main",credentialsId: 'git', url: "$GITHUB_REPO"

                }
            }
        }

        
        stage('check connection') {
            steps {
                sshagent(credentials: ['keypair.pem']) {
                    sh 'ssh -o StrictHostKeyChecking=no $SERVER_USERNAME@$SERVER_IP "mkdir p" '

                }
            }
        }

        stage('copy files') {
            steps {
                sshagent(credentials: ['keypair.pem']) {
                     script {
                        sh 'scp -r ~/workspace/first   $SERVER_USERNAME@$SERVER_IP:~'
                    }
                }
            }
        }

        stage('docker-compose up and down') {
            steps {
                script {
                sh 'ssh -o StrictHostKeyChecking=no $SERVER_USERNAME@$SERVER_IP "cd first && docker-compose down && docker-compose up -d"'
                }
            }
        }

    
        
        stage('Publish to S3') {
            steps {
                sh "aws s3 cp website.html s3://${S3_BUCKET}/ "
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}


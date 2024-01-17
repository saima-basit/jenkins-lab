pipeline {
    agent any

    environment {
        GITHUB_REPO = 'https://github.com/saima-basit/jenkins-lab.git'
        S3_BUCKET = 'saimasbucket'
        SERVER_IP = '3.27.136.237'
        SERVER_USERNAME = 'ubuntu'
        APP_NAME = 'website.html'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: GITHUB_REPO
            }
        }

        stage('de allocate previous port') {
            steps {
                script {
                    sh 'sudo docker stop python-flask-container || true'
                    sh 'sudo docker rm python-flask-container || true'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t python-flask-app .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh 'docker run -d -p 1234:5000 python-flask-app'
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


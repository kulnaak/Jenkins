pipeline {
    agent any
    tools {
        git "GIT"
    }
    stages {
        stage('Clone repository') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/kulnaak/Jenkins.git']])
            }
        }
        stage('Build Docker image') {
            steps {
                sh 'sudo docker build -t demo-nodejs-web:latest .'
            }
        }
        stage('Deploy Docker image') {
            steps {
                script {
                def containerExists = sh(script: 'sudo docker ps -a | grep demo-nodejs-web-service', returnStatus: true) == 0
                if (containerExists) {
                    sh 'sudo docker stop demo-nodejs-web-service'
                    sh 'sudo docker rm demo-nodejs-web-service'
                }
                }
                sh 'sudo docker run -p 5000:5000 -d --name demo-nodejs-web-service demo-nodejs-web:latest'
            }
        }
    }
}

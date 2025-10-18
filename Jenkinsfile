pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-login')
    }
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/yeheskhielprasetyo123/simple-node-app-2.git'
            }
        }
        stage('Build Image') {
            steps {
                sh 'docker build -t yeheskhiel/simple-node-app-2:latest .'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                sh '''
                echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                docker push yeheskhiel/simple-node-app-2:latest
                '''
            }
        }
        stage('Deploy to Server 2') {
            steps {
                sh '''
                ssh -o StrictHostKeyChecking=no eki@10.51.96.190 "
                docker pull yeheskhiel/simple-node-app:latest &&
                docker stop simple-node-app || true &&
                docker rm simple-node-app || true &&
                docker run -d -p 3000:3000 --name simple-node-app-2 yeheskhiel/simple-node-app:latest
                "
                '''
            }
        }
    }
}

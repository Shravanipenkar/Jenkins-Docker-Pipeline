pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Fetching source code from GitHub'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker Image'
                bat 'docker build -t jenkins-web-app:%BUILD_NUMBER% .'
            }
        }

        stage('Deploy Application') {
            steps {
                echo 'Deploying application using Docker'

                bat 'docker rm -f jenkins-web-app 2>NUL || exit /b 0'

                bat 'docker run -d --name jenkins-web-app -p 8081:80 --restart unless-stopped jenkins-web-app:%BUILD_NUMBER%'
            }
        }

        stage('Verify Deployment') {
            steps {
                bat 'docker ps'
            }
        }
    }

    post {
        success {
            echo 'Application deployed successfully!'
        }

        failure {
            echo 'Pipeline execution failed!'
        }
    }
}
pipeline {

    agent any

    stages {

        stage('Checkout Code') {
            steps {
                echo 'Pulling latest code...'
                checkout scm
            }
        }

        stage('Stop Old Containers') {
            steps {
                echo 'Stopping old containers...'
                sh 'docker compose down'
            }
        }

        stage('Build Docker Images') {
            steps {
                echo 'Building Docker images...'
                sh 'docker compose build'
            }
        }

        stage('Deploy Application') {
            steps {
                echo 'Starting containers...'
                sh 'docker compose up -d'
            }
        }

        stage('Check Application Status') {
            steps {
                echo 'Checking running containers...'
                sh 'docker ps'
            }
        }

    }

    post {

        success {
            echo 'Deployment Successful 🚀'
        }

        failure {
            echo 'Deployment Failed ❌'
        }

    }
}

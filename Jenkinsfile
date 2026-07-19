pipeline {

    agent any

    stages {

        stage('Checkout Code') {
            steps {
                echo 'Pulling latest code...'
                checkout scm
            }
        }


        stage('Create Environment File') {
            steps {
                echo 'Creating backend .env file...'

                withCredentials([
                    string(credentialsId: 'MONGODB_URI', variable: 'MONGODB_URI'),
                    string(credentialsId: 'JWT_SECRET', variable: 'JWT_SECRET'),
                    string(credentialsId: 'CLOUDINARY_CLOUD_NAME', variable: 'CLOUDINARY_CLOUD_NAME'),
                    string(credentialsId: 'CLOUDINARY_API_KEY', variable: 'CLOUDINARY_API_KEY'),
                    string(credentialsId: 'CLOUDINARY_API_SECRET', variable: 'CLOUDINARY_API_SECRET')
                ]) {

                    sh '''
                    cat > backend/.env <<EOF
MONGODB_URI=$MONGODB_URI
PORT=8000
JWT_SECRET=$JWT_SECRET

CLIENT_URL=http://3.145.26.9:5180

CLOUDINARY_CLOUD_NAME=$CLOUDINARY_CLOUD_NAME
CLOUDINARY_API_KEY=$CLOUDINARY_API_KEY
CLOUDINARY_API_SECRET=$CLOUDINARY_API_SECRET

NODE_ENV=production
EOF
                    '''
                }
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

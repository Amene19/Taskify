pipeline {
    agent any

    stages {
        stage('Build & Test') {
            steps {
                echo 'Building and testing the project...'
                bat 'mvn clean install'
            }
        }
        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'
                bat 'docker build -t taskify-backend:latest .'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
    }
}



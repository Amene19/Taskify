pipeline {
    agent any

    stages {
        stage('Build & Test') {
            steps {
                echo 'Building and testing the project...'
                sh 'mvn clean install'
            }
        }
        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t taskify-backend:latest .'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
    }
}



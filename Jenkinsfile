pipeline {
    agent any
    
    tools {
        maven 'Maven-3.9'
    }
    
    options {
        timestamps()
        timeout(time: 1, unit: 'HOURS')
        buildDiscarder(logRotator(numToKeepStr: '10'))
    }
    
    environment {
        APP_NAME = 'taskify-backend'
        DOCKER_REGISTRY = 'docker.io'
        DOCKER_IMAGE_NAME = "${DOCKER_REGISTRY}/${DOCKER_USERNAME}/${APP_NAME}"
        SONAR_PROJECT_KEY = 'taskify-backend'
        SLACK_CHANNEL = '#builds'
    }
    
    stages {
        stage('Checkout') {
            steps {
                script {
                    echo "📦 Checking out source code from SCM..."
                }
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                script {
                    echo "🔨 Building the Spring Boot application..."
                }
                sh 'mvn clean compile'
            }
        }
        
        stage('Test') {
            steps {
                script {
                    echo "🧪 Running unit tests with JUnit..."
                }
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/**/*.xml'
                    script {
                        echo "✅ Test reports generated"
                    }
                }
            }
        }
        
        stage('Code Coverage & Analysis') {
            steps {
                script {
                    echo "📊 Running code coverage analysis with JaCoCo..."
                }
                sh 'mvn jacoco:report'
                script {
                    echo "✅ Coverage report generated in target/site/jacoco/index.html"
                }
            }
        }
        
        stage('SonarQube Analysis') {
            when {
                branch 'main'
            }
            steps {
                script {
                    echo "🔍 Performing SonarQube code quality analysis..."
                    // Uncomment when SonarQube is configured
                    // sh '''
                    //     mvn sonar:sonar \
                    //       -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                    //       -Dsonar.sources=src/main/java \
                    //       -Dsonar.tests=src/test/java \
                    //       -Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml
                    // '''
                    echo "ℹ️ SonarQube stage (commented out - configure credentials first)"
                }
            }
        }
        
        stage('Package') {
            steps {
                script {
                    echo "📦 Packaging application as JAR..."
                }
                sh 'mvn package -DskipTests'
            }
            post {
                success {
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    echo "🐳 Building Docker image..."
                    // Commented for security - uncomment when Docker credentials are set up
                    // docker.build("${DOCKER_IMAGE_NAME}:${BUILD_NUMBER}")
                    // docker.build("${DOCKER_IMAGE_NAME}:latest")
                    echo "ℹ️ Docker build stage (commented out - configure Docker credentials first)"
                }
            }
        }
        
        stage('Push to Registry') {
            when {
                branch 'main'
            }
            steps {
                script {
                    echo "📤 Pushing Docker image to registry..."
                    // Commented for security - uncomment when Docker Hub credentials are configured
                    // docker.image("${DOCKER_IMAGE_NAME}:${BUILD_NUMBER}").push()
                    // docker.image("${DOCKER_IMAGE_NAME}:latest").push()
                    echo "ℹ️ Push stage (commented out - configure Docker credentials first)"
                }
            }
        }
    }
    
    post {
        always {
            script {
                echo "🧹 Cleaning up workspace..."
                cleanWs()
            }
        }
        
        success {
            script {
                echo "✅ Pipeline execution SUCCESS!"
                // Uncomment when Slack integration is configured
                // slackSend(
                //     channel: "${SLACK_CHANNEL}",
                //     color: 'good',
                //     message: "✅ Build SUCCESS: ${APP_NAME} #${BUILD_NUMBER}\nBranch: ${GIT_BRANCH}\nAuthor: ${GIT_AUTHOR}",
                //     webhookUrl: "${SLACK_WEBHOOK}"
                // )
            }
        }
        
        failure {
            script {
                echo "❌ Pipeline execution FAILED!"
                // Uncomment when Slack integration is configured
                // slackSend(
                //     channel: "${SLACK_CHANNEL}",
                //     color: 'danger',
                //     message: "❌ Build FAILED: ${APP_NAME} #${BUILD_NUMBER}\nBranch: ${GIT_BRANCH}\nCheck console output: ${BUILD_URL}console",
                //     webhookUrl: "${SLACK_WEBHOOK}"
                // )
            }
        }
        
        unstable {
            script {
                echo "⚠️ Pipeline execution is UNSTABLE"
            }
        }
    }
}

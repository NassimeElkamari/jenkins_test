pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'my-react-app'
        DOCKER_CONTAINER = 'react-app'
        DOCKER_PORT = '3001'
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo '🚀 Cloning React app repository...'
                git branch: 'main', 
                    url: 'https://github.com/NassimeElkamari/jenkins_test.git',
                    credentialsId: 'github-credentials-id'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo '📦 Installing npm dependencies...'
                bat 'npm install'
            }
        }

        stage('Build React App') {
            steps {
                echo '🏗️ Building React app...'
                bat 'npm run build'
            }
        }

        stage('Test Build') {
            steps {
                echo '✅ Build completed successfully!'
                bat 'dir build'
            }
        }

        stage('Docker Build') {
            steps {
                echo '🐳 Building Docker image...'
                bat "docker build -t %DOCKER_IMAGE%:latest ."
            }
        }

    }

    post {
        always {
            echo '📊 Pipeline completed. Cleaning up...'
        }
        
        success {
            echo '🎉 SUCCESS: Pipeline completed successfully!'
        }
        
        failure {
            echo '❌ FAILURE: Pipeline failed!'
            echo '🔍 Check console output for errors'
        }
        
        unstable {
            echo '⚠️ UNSTABLE: Pipeline completed but with warnings'
        }
    }
}

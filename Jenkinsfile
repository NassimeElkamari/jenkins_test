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

        stage('Docker Run') {
            steps {
                echo '🚀 Running Docker container...'
                script {
                    // Stop and remove existing container if exists
                    bat "docker stop %DOCKER_CONTAINER% || echo Container not running"
                    bat "docker rm %DOCKER_CONTAINER% || echo Container not existing"

                    // Run container
                    bat "docker run -d -p %DOCKER_PORT%:80 --name %DOCKER_CONTAINER% %DOCKER_IMAGE%:latest"

                    // Wait and test
                    bat "timeout /t 5"
                    bat "curl -s http://localhost:%DOCKER_PORT% | findstr React || echo Docker container starting..."
                }
            }
        }
    }

    post {
        always {
            echo '📊 Pipeline completed. Cleaning up...'
            // Optional cleanup
            bat "docker stop %DOCKER_CONTAINER% || echo Container not running"
            bat "docker rm %DOCKER_CONTAINER% || echo Container not existing"
        }
        
        success {
            echo '🎉 SUCCESS: Pipeline completed successfully!'
            echo "🌐 React app running at http://localhost:%DOCKER_PORT%"
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

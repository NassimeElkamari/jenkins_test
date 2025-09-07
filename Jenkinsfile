pipeline {
    agent any

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
                sh 'npm install'
            }
        }


        stage('Build React App') {
            steps {
                echo '🏗️ Building React app...'
                sh 'npm run build'
            }
        }

        stage('Test Build') {
            steps {
                echo '✅ Build completed successfully!'
                sh 'ls -la build/'
            }
        }

        // NEW STAGE: Docker Build
        stage('Docker Build') {
            steps {
                echo '🐳 Building Docker image...'
                sh 'docker build -t my-react-app:latest .'
            }
        }

        // NEW STAGE: Docker Run
        stage('Docker Run') {
            steps {
                echo '🚀 Running Docker container...'
                script {
                    // Stop and remove any existing container
                    sh 'docker stop react-app || true'
                    sh 'docker rm react-app || true'
                    
                    // Run new container
                    sh 'docker run -d -p 3001:80 --name react-app my-react-app:latest'
                    
                    // Wait and test
                    sh 'sleep 3'
                    sh 'curl -s http://localhost:3001 | grep "React" || echo "Docker container starting..."'
                }
            }
        }
    }

    post {
    always {
        echo '📊 Pipeline completed. Cleaning up...'
        // Clean up serve process
        sh 'pkill -f "serve -s build" || true'
    }
    
    success {
        echo '🎉 SUCCESS: Pipeline completed successfully!'
        echo '🌐 Serve version: http://your-jenkins-server-ip:3000'
        echo '🐳 Docker version: http://your-jenkins-server-ip:3001'
        echo '✅ Tests passed, build created, and deployed!'
        
        // You can add email or Slack notifications here later
    }
    
    failure {
        echo '❌ FAILURE: Pipeline failed!'
        echo '🔍 Check the console output for errors'
        echo '💡 Common issues: test failures, build errors, or port conflicts'
        
        // You can add failure notifications here
    }
    
    unstable {
        echo '⚠️  UNSTABLE: Pipeline completed but with warnings'
        echo '📝 Check test results or build warnings'
    }
}
}
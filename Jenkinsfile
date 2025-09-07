pipeline {
    agent any

    stages {
        stage('Clone and Setup') {
            steps {
                git branch: 'main', 
                url: 'https://github.com/YOUR_USERNAME/my-simple-react-app.git',
                credentialsId: 'github-credentials-id'
                
                sh 'npm install'
            }
        }

        stage('Build React App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Test Build') {
            steps {
                echo '✅ Build completed successfully!'
                sh 'ls -la build/'
            }
        }
    }
}
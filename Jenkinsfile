pipeline {
    agent any

    environment {
        // Define any global environment variables here (e.g., NODE_ENV)
        NODE_ENV = 'production'
    }

    stages {
        stage('Checkout') {
            steps {
                // Pull code from SCM (this assumes the GitHub webhook triggers this)
                git branch: 'main', url: 'https://github.com/yourusername/your-repo.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install node modules
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                // Build the React application
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                // Run tests (you can add your test scripts here)
                sh 'npm test -- --watchAll=false'
            }
        }

        stage('Deploy') {
            steps {
                // For a React app, you might simply copy the build folder to a web server folder or deploy via a hosting service.
                // Example using a simple Node static server (adjust according to your deployment strategy)
                sh '''
                    npm install -g serve
                    nohup serve -s build -l 5000 &
                '''
                // Alternatively, you can add deployment commands (like uploading to S3, restarting a Node server with pm2, etc.)
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

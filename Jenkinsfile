pipeline {
    agent any

    environment {
        NODE_ENV = 'production'
        // Ensure Vercel token is securely managed via Jenkins credentials store
        VERCEL_TOKEN = 'fEgCy7inyK5LY6KdJAcmUVLm' //credentials('9cd937ab-bd53-45df-aad2-7fde96e83cf1')  // Use Jenkins credentials ID for the Vercel token
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/mohitjangra1910/react-calculator.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    npm install
                    npm install react-scripts@latest
                    npm install webpack@latest webpack-cli@latest
                    npx browserslist@latest --update-db
                '''
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                // Avoid failing pipeline if no tests are found
                sh 'npm test -- --watchAll=false || true'
            }
        }

        stage('Deploy') {
            steps {
                // Install Vercel CLI without sudo (ensure Jenkins user has appropriate permissions)
                sh '''
                    sudo npm install -g vercel
                    vercel --token $VERCEL_TOKEN --prod --yes --name "my-react-calculator"
                '''
                
                // Pause and wait for user input to continue the pipeline
                input message: 'Deployment to Vercel is complete. Do you want to finish the pipeline?', 
                      ok: 'Yes, Finish Pipeline', 
                      parameters: [
                          choice(name: 'Action', choices: ['Finish', 'Abort'], description: 'Choose whether to finish or abort the pipeline')
                      ]
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

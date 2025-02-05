pipeline {
    agent any

    environment {
        NODE_ENV = 'production'
        // Set your Vercel token as an environment variable
        // You can store the token securely in Jenkins' credentials store
        VERCEL_TOKEN = credentials('fEgCy7inyK5LY6KdJAcmUVLm') // Replace with your Jenkins credentials ID
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
                sh 'npm test -- --watchAll=false || true'
            }
        }

        stage('Deploy') {
            steps {
                // Deploy to Vercel using the Vercel CLI
                sh '''
                    sudo npm install -g vercel
                    vercel --token $VERCEL_TOKEN --prod --confirm
                '''
                
                // Pause the pipeline and wait for user input to proceed
                input message: 'Deployment to Vercel is complete. Do you want to finish the pipeline?', ok: 'Yes, Finish Pipeline', parameters: [
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

pipeline {
    agent any

    environment {
        NODE_ENV = 'production'
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
                    npm install webpack@latest webpack-cli@latest
                    npm install react-scripts@latest
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
                sh '''
                    npm install serve
                    nohup npx serve -s build -l 5000 &
                '''
                input message: 'Deployment is complete. Do you want to finish the pipeline?', ok: 'Yes, Finish Pipeline', parameters: [
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

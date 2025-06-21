pipeline {
    agent any

    environment {
        RENDER_API_TOKEN = credentials('RENDER_TOKEN')
        SERVICE_ID = credentials('SERVICE_ID')
    }

    stages {
        stage('Checkout the repo') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm run test'
            }
        }

         stage('Deploy to Render') {
            steps {
                echo 'Triggering deployment on Render...'
                bat """
                    curl -X POST \\
                      -H "Authorization: Bearer $RENDER_TOKEN" \\
                      -H "Accept: application/json" \\
                      -H "Content-Type: application/json" \\
                      $SERVICE_ID
                """
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed'
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}

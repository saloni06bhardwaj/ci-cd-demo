pipeline {
    agent any

    tools {
        nodejs "nodejs"  // Make sure 'nodejs' is defined in Jenkins Global Tool Config
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
                sh 'ls -la'  // For debugging
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run App') {
            steps {
                sh 'npm start'
            }
        }
    }

    post {
        success {
            echo "✅ CI/CD pipeline executed successfully!"
        }
        failure {
            echo "❌ CI/CD pipeline failed."
        }
    }
}

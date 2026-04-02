pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install --legacy-peer-deps'
            }
        }
        stage('Test') {
            steps {
                // Run tests in headless mode for Jenkins
                sh 'npx npx npm run test:headless'
            }
        }
        stage('Build') {
            steps {
                sh 'ng build --configuration production'
            }
        }
    }
}

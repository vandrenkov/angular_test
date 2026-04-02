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
                sh 'npx ng test --watch=false --browsers=ChromeHeadless -- --no-sandbox --disable-gpu'
            }
        }
        stage('Build') {
            steps {
                // Use npx so Jenkins can find the 'ng' command
                sh 'npx ng build --configuration production'
            }
        }
    }
}

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
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                // Run tests in headless mode for Jenkins
                sh 'npx ng test --watch=false --browsers=ChromeHeadless'
            }
        }
        stage('Build') {
            steps {
                sh 'npx ng build --configuration production'
            }
        }
    }
}

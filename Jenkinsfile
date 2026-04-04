pipeline {
    agent {
        docker { image 'trion/ng-cli-karma:latest' }
    }

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
                // sh 'npx ng test --watch=false --browsers=ChromeHeadless -- --no-sandbox --disable-gpu'
                // This is cleaner and less prone to syntax errors
                sh 'npm run test:ci'
            }
        }
        stage('Build') {
            steps {
                sh 'npx ng build --configuration production'
            }
        }
    }
}

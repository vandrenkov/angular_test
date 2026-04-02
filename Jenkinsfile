pipeline {
    agent {
        docker { 
            image 'trion/ng-cli-karma:latest' 
            // This image has Node, Angular CLI, and Chrome pre-installed
        }
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
                sh 'npx npm run test:headless'
            }
        }
        stage('Build') {
            steps {
                sh 'ng build --configuration production'
            }
        }
    }
}

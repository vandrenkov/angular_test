// Requires Node.js + npm on the agent (install in the Jenkins controller image, or use an agent with Node).
// To use "agent { docker { image '...' } }" instead, install the "Docker Pipeline" plugin and ensure
// the Jenkins agent can run Docker (docker.sock mount or Docker-in-Docker).
pipeline {
    agent any

      tools {
        nodejs 'node20' // This must match the name you gave in Global Tool Configuration
    }

    options {
        timestamps()
        timeout(time: 30, unit: 'MINUTES')
    }

    environment {
        CI = 'true'
        // Fixed path under the workspace so cache purge / scheduled jobs can target the same tree as builds.
        PUPPETEER_CACHE_DIR = "${env.WORKSPACE}/.cache/puppeteer"
    }

    stages {
        stage('Initialization') {
            steps {
                sh '''
                    echo "WORKSPACE=${WORKSPACE}"
                    echo "PUPPETEER_CACHE_DIR=${PUPPETEER_CACHE_DIR}"
                '''
            }
        }
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'node -v && npm -v'
                sh 'npm ci --legacy-peer-deps'
            }
        }
        stage('Test') {
            steps {
                sh 'npm run test:ci'
            }
        }
        stage('Build') {
            steps {
                sh 'npx ng build test_jenkins --configuration production'
            }
        }
    }
}

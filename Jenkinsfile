// Requires Node.js + npm on the agent (install in the Jenkins controller image, or use an agent with Node).
// To use "agent { docker { image '...' } }" instead, install the "Docker Pipeline" plugin and ensure
// the Jenkins agent can run Docker (docker.sock mount or Docker-in-Docker).
pipeline {
    agent any

    tools {
        nodejs 'node20' // Match Global Tool Configuration name (same as angular.jenkins.solution)
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
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Initialization') {
            steps {
                sh '''
                    echo "WORKSPACE=${WORKSPACE}"
                    echo "PUPPETEER_CACHE_DIR=${PUPPETEER_CACHE_DIR}"
                    df -h . || true
                '''
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'node -v && npm -v'
                // Skip Puppeteer postinstall download during npm ci; install browser once into PUPPETEER_CACHE_DIR.
                sh '''
                    export PUPPETEER_SKIP_DOWNLOAD=true
                    npm ci --legacy-peer-deps
                '''
                sh 'npx puppeteer browsers install chrome'
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

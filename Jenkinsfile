pipeline {
    agent {
        docker {
            image 'node:18.17.1-alpine3.18'
            args '-p 3000:3000'
        }
    }
    environment {
        NPM_CONFIG_LOGLEVEL = 'error'
        NODEJS_INSTALL     = 'NodeJS 18.x'
    }
    stages {
        stage('install ui packages') {
            steps {
                nodejs(nodeJSInstallationName: "${NODEJS_INSTALL}") {
                sh 'node --version'
                sh 'npm --version'
                sh 'npm ci'
                }
            }
        }
        // stage('Build') {
        //     steps {
        //         sh 'npm ci'
        //     }
        // }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}

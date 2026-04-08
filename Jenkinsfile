pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git credentialsId: '611b6d29-4688-45b3-80aa-31bfcf23fa61', url: 'https://github.com/tc-bilal/simple-web-app-main'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing Node.js dependencies...'
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                echo 'Build step (if needed)'
                // For Node.js, often build is optional, otherwise: sh 'npm run build'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Get SonarScanner installation from Jenkins
                    def scannerHome = tool 'SonarScanner'

                    // Run SonarQube scan
                    withSonarQubeEnv('SonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
    }
}

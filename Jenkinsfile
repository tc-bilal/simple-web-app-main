pipeline {
    agent any

    options {
        skipDefaultCheckout() // prevents Jenkins from double-checking out
    }

    stages {

        stage('Checkout') {
            steps {
                // Checkout your repo on the main branch
                git branch: 'main',
                    credentialsId: '611b6d29-4688-45b3-80aa-31bfcf23fa61',
                    url: 'https://github.com/tc-bilal/simple-web-app-main'
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
                // Optional: sh 'npm run build' if your app has a build step
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Use SonarScanner installed in Jenkins
                    def scannerHome = tool 'SonarScanner'

                    // Run SonarQube analysis
                    withSonarQubeEnv('SonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
    }
}

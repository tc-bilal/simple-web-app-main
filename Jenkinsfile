pipeline {
    agent any

    options {
        skipDefaultCheckout() // prevents Jenkins from double-checking out
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: '611b6d29-4688-45b3-80aa-31bfcf23fa61',
                    url: 'https://github.com/tc-bilal/simple-web-app-main'
            }
        }

        stage('Build') {
            steps {
                echo 'No build step needed for this repo'
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

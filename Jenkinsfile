pipeline {
    agent any
    options { skipDefaultCheckout() }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/tc-bilal/simple-web-app-main.git'
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
                    def scannerHome = tool 'SonarScanner'
                    withSonarQubeEnv('SonarQube') {
                        sh """
                        ${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=simple-web-app \
                        -Dsonar.projectName="Simple Web App" \
                        -Dsonar.sources=.
                        """
                    }
                }
            }
        }

        stage('AppScan Analysis') {
            agent { label 'windows-appscan' }  // Ensures this stage runs only on a Windows node
            steps {
                script {
                    def appScanPath = '"C:\\Program Files (x86)\\HCL\\AppScan Standard\\AppScanCMD.exe"'
            def destFolder = "${WORKSPACE}\\AppScanResults"

            // Ensure destination folder exists
            bat "if not exist \"${destFolder}\" mkdir \"${destFolder}\""

            echo "Running HCL AppScan on Windows node..."
            bat "${appScanPath} exec /surl \"http://localhost:8887/\" /dest_scan \"${destFolder}\" /verbose"
                }
            }
        }
    }
}

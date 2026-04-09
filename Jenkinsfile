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
    agent { label 'windows-appscan' }
        steps {
            script {
                def appScanPath = '"C:\\Program Files (x86)\\HCL\\AppScan Standard\\AppScanCMD.exe"'
                def workspaceResults = "${WORKSPACE}\\AppScanResults"
                def templatePath = '"C:\\Program Files (x86)\\HCL\\AppScan Standard\\Templates\\ProductionSite.scant"'
                def reportFile = "${workspaceResults}\\AppScanReport.xml"
    
                // Ensure results folder exists
                bat "if not exist ${workspaceResults} mkdir ${workspaceResults}"
    
                echo "Running HCL AppScan and generating report in workspace..."
    
                // Generate XML report directly
                bat "${appScanPath} exec /stemplate ${templatePath} /surl http://localhost:8887/ /report_file ${reportFile} /report_type Xml /verbose"
    
                echo "AppScan completed. Report saved at ${reportFile}"
                }
            }
        }
    }
}

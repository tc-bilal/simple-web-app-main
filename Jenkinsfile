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

    stage('Docker Build & Run') {
    steps {
        bat '''
        docker build -t test/app:latest .

        docker stop test-app || exit 0
        docker rm test-app || exit 0

        docker run -d -p 8887:8887 --name test-app test/app:latest
        '''
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
    
                // Generate timestamp for unique report file
                def timestamp = new Date().format("yyyyMMdd_HHmmss")
                def reportFile = "${workspaceResults}\\AppScanReport_${timestamp}.pdf"  // PDF for graphical output
    
                // Ensure results folder exists
                bat "if not exist ${workspaceResults} mkdir ${workspaceResults}"
    
                echo "Running HCL AppScan and generating graphical report..."
    
                bat "${appScanPath} exec /stemplate ${templatePath} /surl http://192.168.1.9:8887/ /report_file ${reportFile} /report_type Pdf /verbose"
    
                echo "AppScan completed. PDF report saved at ${reportFile}"
    
                }
            }
        }
    }
}

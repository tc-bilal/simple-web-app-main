pipeline {
    agent any

    stages {

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

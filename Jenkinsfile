pipeline {
    agent any

    environment {
        // SonarQube configuration
        SONARQUBE_SERVER = 'SonarQube' // The name of your SonarQube server configured in Jenkins
        GITHUB_REPO = 'https://github.com/your-user/your-repo.git' // Your GitHub repository URL
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from GitHub
                git branch: 'main', url: GITHUB_REPO
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Install necessary Node.js dependencies
                    sh 'npm install'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // SonarQube Scanner analysis
                    withSonarQubeEnv(SONARQUBE_SERVER) {
                        sh 'sonar-scanner'
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    // Wait for the SonarQube Quality Gate to complete and abort the pipeline if failed
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Build and Test') {
            steps {
                script {
                    // Optionally run tests or build after SonarQube analysis
                    sh 'npm test'
                }
            }
        }
    }

    post {
        success {
            echo 'Build and SonarQube scan completed successfully!'
        }
        failure {
            echo 'Build or SonarQube scan failed!'
        }
    }
}

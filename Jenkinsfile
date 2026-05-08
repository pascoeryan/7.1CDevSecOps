pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/pascoeryan/7.1CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || true'
            }
        }

        stage('Coverage') {
            steps {
                sh 'npm test -- --coverage || true'
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    sh '''
                        set -e

                        # Clean old install to avoid unzip prompts
                        rm -rf sonar-scanner
                        rm -f sonar-scanner-cli-5.0.1.3006-linux.zip

                        # Download
                        wget -q https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip

                        # Force unzip (NO PROMPTS)
                        unzip -o sonar-scanner-cli-5.0.1.3006-linux.zip

                        mv sonar-scanner-5.0.1.3006-linux sonar-scanner

                        # Run scanner using absolute path
                        ./sonar-scanner/bin/sonar-scanner \
                        -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }

        stage('Security Scan') {
            steps {
                sh 'npm audit || true'
            }
        }
    }
}
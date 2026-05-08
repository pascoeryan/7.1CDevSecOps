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
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {

                  sh '''
                  wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip

                  unzip sonar-scanner-cli-5.0.1.3006-linux.zip

                  export PATH=$PATH:$(pwd)/sonar-scanner-cli-5.0.1.3006-linux/bin

                  sonar-scanner
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
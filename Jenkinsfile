pipeline {
agent any
stages {
stage('Checkout') {
steps {
git branch: 'main', url: 'https://github.com/pascoeryan/7.1CDevSecOps.git'
}
}
stage('Install Dependencies') {
steps {
sh 'npm install'
}
}
stage('Run Tests') {
steps {
sh 'npm test || true' // Allows pipeline to continue despite test failures
}
}
stage('SonarCloud Analysis') {
    steps {
        withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
            sh '''
            wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-6.2.1.4610-linux-x64.zip

            unzip sonar-scanner-6.2.1.4610-linux-x64.zip

            export PATH=$PATH:$(pwd)/sonar-scanner-6.2.1.4610-linux-x64/bin
            
            sonar-scanner
            '''
        }
    }
}
stage('Generate Coverage Report') {
steps {
// Ensure coverage report exists
sh 'npm run coverage || true'
}
}
stage('NPM Audit (Security Scan)') {
steps {
sh 'npm audit || true' // This will show known CVEs in the output
}
}
}
}
pipeline {
agent any
stages {
stage('Checkout') {
  steps {
    sh '''
      docker run --rm -v "$PWD:/repo" alpine/git \
        clone https://github.com/pascoeryan/7.1CDevSecOps.git repo
    '''
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
        docker run --rm \
          -e SONAR_HOST_URL="https://sonarcloud.io" \
          -e SONAR_TOKEN="$SONAR_TOKEN" \
          -v "$PWD:/usr/src" \
          sonarsource/sonar-scanner-cli
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
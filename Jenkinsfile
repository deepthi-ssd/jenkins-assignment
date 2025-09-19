pipeline {
  agent {
    docker {
      image 'node:18-alpine'
      args '--rm' 
    }
  }
  stages {
    stage('Checkout') { steps { checkout scm } }
    stage('Install')  { steps { sh 'npm install' } }
    stage('Test')     { steps { sh 'npm test' } }
    stage('Build')    { steps { sh 'npm run build' } }
    stage('Archive & Publish') {
      steps {
        archiveArtifacts artifacts: 'dist/**', fingerprint: true
        publishHTML (target: [
          allowMissing: false,
          alwaysLinkToLastBuild: true,
          keepAll: true,
          reportDir: 'dist',
          reportFiles: 'index.html',
          reportName: 'Application Report'
        ])
      }
    }
  }
  post {
    success { echo 'Pipeline succeeded' }
    failure { echo 'Pipeline failed' }
  }
}

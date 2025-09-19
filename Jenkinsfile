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


post {
    success {
        emailext(
            to: 'raimund@rittnauer.at',
            subject: "✅ SUCCESS: Jenkins Build #${env.BUILD_NUMBER}",
            body: """Hello Team,

The build succeeded for job: ${env.JOB_NAME}
Build number: ${env.BUILD_NUMBER}
Build URL: ${env.BUILD_URL}

Regards,
Jenkins
"""
        )
    }
    failure {
        emailext(
            to: 'raimund@rittnauer.at',
            subject: "❌ FAILURE: Jenkins Build #${env.BUILD_NUMBER}",
            body: """Hello Team,

The build failed for job: ${env.JOB_NAME}
Build number: ${env.BUILD_NUMBER}
Build URL: ${env.BUILD_URL}

Please check the console output for details.

Regards,
Jenkins
"""
        )
    }
}




pipeline {
    agent {
        docker {
            image 'node:18-alpine'
            args '--rm'
        }
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Install') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        stage('Archive & Publish') {
            steps {
                archiveArtifacts artifacts: 'dist/**', fingerprint: true
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'dist',
                    reportFiles: 'index.html',
                    reportName: 'Application Report'
                ])
                publishHTML(target: [
                    allowMissing: true,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'dist',
                    reportFiles: 'build-report.html',
                    reportName: 'Build Report'
                ])
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded'
            emailext(
                subject: "✅ Build Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "The build for ${env.JOB_NAME} (#${env.BUILD_NUMBER}) succeeded.\nCheck console output at ${env.BUILD_URL}",
                to: 'raimund@rittnauer.at'
            )
        }
        failure {
            echo 'Pipeline failed'
            emailext(
                subject: "❌ Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "The build for ${env.JOB_NAME} (#${env.BUILD_NUMBER}) failed.\nCheck console output at ${env.BUILD_URL}",
                to: 'raimund@rittnauer.at'
            )
        }
    }
}



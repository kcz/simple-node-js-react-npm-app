pipeline {
    agent {
        docker {
            image 'node:8.13.0-jessie'
            args '-p 3001:3000'
        }
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                sh 'yarn install'
                sh 'yarn build'
                archiveArtifacts artifacts: 'build/**/*.*', fingerprint: true
            }
        }
        stage('Test') {
            steps {
                withEnv(["JEST_JUNIT_OUTPUT=./jest-test-results.xml"]) {
                    sh 'yarn test --reporters=default --reporters=jest-junit'
                }
                junit '*.xml'
            }
        }
    }
    post {
        always {
            publishHTML(
                target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: false,
                    keepAll: true,
                    reportDir: 'build',
                    reportFiles: 'index.html',
                    reportName: "SiteEntry"
                ]
            )
        }
    }
}

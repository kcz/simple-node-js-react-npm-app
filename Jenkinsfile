pipeline {
    agent {
        docker {
            image 'node:8.13.0-jessie'
            args '-p 3000:3000'
        }
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                sh 'yarn install'
            }
        }
        stage('Test') {
            withEnv(["JEST_JUNIT_OUTPUT=./jest-test-results.xml"]) {
                sh 'yarn test -- --ci --testResultsProcessor="jest-junit"'
            }
            junit 'jest-test-results.xml'
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}

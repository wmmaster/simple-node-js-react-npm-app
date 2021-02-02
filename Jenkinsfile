pipeline {
    agent {
        docker {
            image 'node:6-alpine' 
            args '-p 3000:3000 -u root' 
        }
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('Message') {
            steps {
                sh 'apk add curl'
                sh 'curl -X POST -H \'Content-type: application/json\' --data \'{"text":"Hello, World!"}\' https://hooks.slack.com/services/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'
            }
        }
        stage('Build') { 
            steps {
                sh 'npm install' 
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
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
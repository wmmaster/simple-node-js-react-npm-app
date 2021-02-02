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
        stage('Build') { 
            steps {
                sh 'apk add curl'
                sh 'curl -X POST -H \'Content-type: application/json\' --data @./jenkins/scripts/slack1.json https://hooks.slack.com/services/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'
                sh 'npm install' 
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Certified Deploy') {
            steps {
                sh 'apk add curl'
                input message: 'Request Certified Deployment (Click "Proceed" to continue)'
                sh 'curl -X POST -H \'Content-type: application/json\' --data @./jenkins/scripts/slack1.json https://hooks.slack.com/services/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'
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
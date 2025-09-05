pipeline {
    agent any

    environment {
        SF_AUTH_FILE = "/root/myOrgAuth.txt"
        OWNER_ID = "005dL00000mNOdVQAW"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/NIKHILPESALA1/salesforce-automation.git'
            }
        }

        stage('Create Salesforce Record') {
            steps {
                script {
                    def commitMsg = sh(script: "git log -1 --pretty=%B", returnStdout: true).trim()
                    sh "sf data record create -s myobject__c -v \"Name='${commitMsg}' OwnerId='${OWNER_ID}'\""
                }
            }
        }
    }
}

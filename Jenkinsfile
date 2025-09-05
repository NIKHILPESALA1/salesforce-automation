pipeline {
    agent any

    environment {
        SF_AUTH_FILE = "/root/myOrgAuth.txt"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your-username/your-repo.git'
            }
        }

        stage('Create Salesforce Record') {
            steps {
                script {
                    // Get the latest commit message
                    def commitMsg = sh(script: "git log -1 --pretty=%B", returnStdout: true).trim()
                    
                    // Create Salesforce record
                    sh "sf data record create -s myobject__c -v \"Name='${commitMsg}'\""
                }
            }
        }
    }
}

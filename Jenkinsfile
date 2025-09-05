pipeline {
    agent any

    environment {
        SF_AUTH_FILE = "/root/myOrgAuth.txt"
        OWNER_ID = "005dL00000mNOdVQAW" // Your Salesforce OwnerId
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
                    // Get the latest commit message
                    def commitMsg = sh(script: "git log -1 --pretty=%B", returnStdout: true).trim()
                    
                    // Create Salesforce record using auth file and OwnerId
                    sh """
                        sf auth login sfdx-url --sfdx-url-file \$SF_AUTH_FILE --set-default
                        sf data record create -s myobject__c -v "Name='${commitMsg}' OwnerId='${OWNER_ID}'"
                    """
                }
            }
        }
    }
}

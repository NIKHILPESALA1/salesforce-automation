pipeline {
    agent any

    triggers {
        githubPush()   // Trigger on GitHub push webhook
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Find TXT file') {
            steps {
                script {
                    def txtFiles = sh(script: "git diff-tree --no-commit-id --name-only -r $GIT_COMMIT | grep '.txt$' || true", returnStdout: true).trim()
                    if (txtFiles) {
                        env.TXT_FILE = txtFiles.split('\n')[0]
                        echo "Found text file: ${env.TXT_FILE}"
                    } else {
                        echo "No text file found in this commit. Skipping."
                        currentBuild.result = 'SUCCESS'
                        error("No .txt file detected")
                    }
                }
            }
        }

        stage('Read File Content') {
            when { environment name: 'TXT_FILE', value: '' }
            steps {
                script {
                    env.FILE_CONTENT = sh(script: "cat ${env.TXT_FILE}", returnStdout: true).trim()
                    echo "File content captured"
                }
            }
        }

        stage('Insert into Salesforce') {
            steps {
                script {
                    sh """
                    sf data record create -s Text_Type__c -v "File_Content__c='${FILE_CONTENT}'"
                    """
                }
            }
        }
    }
}

pipeline {
    agent any
    parameters {
        string(name: 'LATEST_TAG', defaultValue: '', description: 'The latest Git tag')
    }
    stages {
        stage('Get Latest Tag') {
            steps {
                script {
                    // Run the Git command to get the latest tag
                    def latestTag = sh(script: 'git describe --tags --abbrev=0', returnStdout: true, returnStatus: true).trim()

                    // Check if the command was successful (status 0 means success)
                    if (latestTag) {
                        echo "Latest tag found: ${latestTag}"
                    } else {
                        echo "No tags found, setting default value"
                        latestTag = "No tags available"
                    }

                    // Set the parameter value (this doesn't change the UI parameter dynamically but sets it in the pipeline)
                    env.LATEST_TAG = latestTag
                }
            }
        }
        stage('Build') {
            steps {
                echo "The latest tag is: ${env.LATEST_TAG}"
                // Use the LATEST_TAG variable for your build steps
            }
        }
    }
}

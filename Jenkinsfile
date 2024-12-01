pipeline {
    agent any
    parameters {
        string(name: 'LATEST_TAG', defaultValue: '', description: 'Latest Git tag')
    }
    stages {
        stage('Checkout Code') {
            steps {
                // Checkout the code from the repository
                checkout scm
            }
        }
        stage('Fetch Latest Tag') {
            steps {
                script {
                    // Get the latest Git tag
                    def latestTag = sh(script: 'git fetch --tags && git describe --tags --abbrev=0', returnStdout: true).trim()
                    
                    // Check if the tag exists and set the default value of the parameter
                    if (latestTag) {
                        currentBuild.description = "Latest Git tag: ${latestTag}"
                        // Set the value of the parameter in the pipeline
                        params.LATEST_TAG = latestTag
                    } else {
                        currentBuild.description = "No Git tags found."
                        params.LATEST_TAG = 'No tags found'
                    }
                }
            }
        }
        stage('Display Selected Tag') {
            steps {
                script {
                    // Display the selected Git tag value
                    echo "Selected Git tag: ${params.LATEST_TAG}"
                }
            }
        }
        stage('Build') {
            steps {
                echo "Building using Git tag: ${params.LATEST_TAG}"
                // Use the tag for your build process as needed
            }
        }
    }
    post {
        always {
            echo "The latest tag used for this build was: ${params.LATEST_TAG}"
        }
    }
}

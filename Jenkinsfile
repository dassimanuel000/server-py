pipeline {
    agent any
    parameters {
        string(name: 'LATEST_TAG', defaultValue: '', description: 'Latest Git tag')
    }
    environment {
        GIT_TAG = ''
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

                    // Check if the tag exists and set the environment variable
                    if (latestTag) {
                        currentBuild.description = "Latest Git tag: ${latestTag}"
                        env.GIT_TAG = latestTag
                    } else {
                        currentBuild.description = "No Git tags found."
                        env.GIT_TAG = 'No tags found'
                    }
                }
            }
        }
        stage('Display Selected Tag') {
            steps {
                script {
                    // Display the selected Git tag value
                    echo "Selected Git tag: ${env.GIT_TAG}"
                }
            }
        }
        stage('Build') {
            steps {
                echo "Building using Git tag: ${env.GIT_TAG}"
                // Use the tag for your build process as needed
            }
        }
    }
    post {
        always {
            echo "The latest tag used for this build was: ${env.GIT_TAG}"
        }
    }
}

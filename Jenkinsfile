pipeline {
    agent any

    environment {
        GIT_TAG = ''
    }

    stages {
        stage('Checkout') {
            steps {
                // Ensure a full clone and fetch tags
                checkout scm
                script {
                    // Fetch tags from the repository
                    sh 'git fetch --tags --force'
                }
            }
        }

        stage('Get Latest Git Tag') {
            steps {
                script {
                    // Attempt to get the latest tag
                    def latestTag = sh(script: 'git describe --tags --abbrev=0', returnStdout: true).trim()
                    
                    // If no tag is found, provide a default value
                    if (!latestTag) {
                        latestTag = "No tags found"
                    }

                    // Set the environment variable and display the tag in the build description
                    env.GIT_TAG = latestTag
                    currentBuild.description = "Latest Git tag: ${latestTag}"

                    echo "Selected Git tag: ${latestTag}"
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Use the fetched tag for the build
                    echo "Building using Git tag: ${env.GIT_TAG}"
                }
            }
        }

        stage('Display Selected Tag') {
            steps {
                echo "The latest tag used for this build was: ${env.GIT_TAG}"
            }
        }
    }

    post {
        always {
            // Clean up actions if necessary
            echo "Build completed with Git tag: ${env.GIT_TAG}"
        }
    }
}

pipeline {
    agent any

    parameters {
        // Define a parameter to allow users to select the Git tag
        choice(name: 'GIT_TAG', choices: ['latest'], description: 'Select the Git tag', defaultValue: 'latest')
    }

    environment {
        FINAL_GIT_TAG = ''
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                script {
                    // Force fetch all tags from the repository
                    sh 'git fetch --tags --force'
                }
            }
        }

        stage('Get Latest Git Tag') {
            steps {
                script {
                    // Fetch the latest tag from the repository
                    def latestTag = sh(script: 'git describe --tags --abbrev=0', returnStdout: true).trim()

                    // If no tag is selected or found, use the latest tag automatically
                    if (params.GIT_TAG == 'latest' && latestTag) {
                        env.FINAL_GIT_TAG = latestTag
                    } else if (params.GIT_TAG != 'latest') {
                        // Use the selected tag from the parameter
                        env.FINAL_GIT_TAG = params.GIT_TAG
                    }

                    echo "Selected Git tag: ${env.FINAL_GIT_TAG}"
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Proceed with the build using the selected/final Git tag
                    echo "Building using Git tag: ${env.FINAL_GIT_TAG}"
                }
            }
        }

        stage('Display Selected Tag') {
            steps {
                echo "The latest tag used for this build was: ${env.FINAL_GIT_TAG}"
            }
        }
    }

    post {
        always {
            echo "Build completed with Git tag: ${env.FINAL_GIT_TAG}"
        }
    }
}

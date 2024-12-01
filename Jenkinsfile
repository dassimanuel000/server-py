pipeline {
    agent any

    parameters {
        choice(name: 'GIT_TAG', choices: [], description: 'Select the Git tag', defaultValue: 'latest')
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

        stage('Get Git Tags') {
            steps {
                script {
                    // Get all tags and split into a list
                    def gitTags = sh(script: 'git tag -l', returnStdout: true).trim().split("\n")
                    // Add 'latest' to the list of tags
                    gitTags = gitTags + ['latest']

                    // Update the choice parameter dynamically
                    currentBuild.displayName = "Git Tag: ${gitTags[0]}"
                    echo "Available Git tags: ${gitTags}"
                    // Update the parameter choice list dynamically (This requires the `Active Choices Plugin`)
                    currentBuild.description = "Select a Git tag from available options"
                }
            }
        }

        stage('Get Latest Git Tag') {
            steps {
                script {
                    def latestTag = sh(script: 'git describe --tags --abbrev=0', returnStdout: true).trim()
                    if (params.GIT_TAG == 'latest' && latestTag) {
                        env.FINAL_GIT_TAG = latestTag
                    } else if (params.GIT_TAG != 'latest') {
                        env.FINAL_GIT_TAG = params.GIT_TAG
                    }

                    echo "Selected Git tag: ${env.FINAL_GIT_TAG}"
                }
            }
        }

        stage('Build') {
            steps {
                script {
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

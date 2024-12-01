pipeline {
    agent any
    parameters {
        string(name: 'LATEST_TAG', defaultValue: 'v0.0.0', description: 'The latest Git tag')
    }
    stages {
        stage('Get Latest Tag') {
            steps {
                script {
                    def latestTag = ''
                    try {
                        latestTag = sh(script: 'git describe --tags --abbrev=0', returnStdout: true).trim()
                    } catch (Exception e) {
                        latestTag = 'v0.0.0' // Fallback to a default tag if no tags exist
                    }
                    echo "The latest tag is: ${latestTag}"
                    env.LATEST_TAG = latestTag
                }
            }
        }
        stage('Build') {
            steps {
                echo "Using tag: ${env.LATEST_TAG}"
                // Continue with your build steps using the latestTag variable
            }
        }
    }
}

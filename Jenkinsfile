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
                    def latestTag = sh(script: 'git describe --tags --abbrev=0', returnStdout: true).trim()

                    // Update the description (optional) or pass it to other stages
                    currentBuild.description = "Latest Git Tag: ${latestTag}"

                    // Set the parameter value (This does not dynamically set the UI parameter but sets a value)
                    echo "Setting latest tag: ${latestTag}"

                    // To use the value in subsequent stages, assign it to a parameter
                    env.LATEST_TAG = latestTag
                }
            }
        }
        stage('Build') {
            steps {
                echo "The latest tag is: ${env.LATEST_TAG}"
                // You can use the LATEST_TAG environment variable in further stages
            }
        }
    }
}

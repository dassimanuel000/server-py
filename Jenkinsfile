pipeline {
    agent any
    parameters {
        // Define Uno Choice Parameter for Latest Tag
        unoChoice(name: 'LATEST_TAG', 
                  description: 'Latest Git tag',
                  choices: { 
                      // This script will fetch the latest tag dynamically
                      def latestTag = sh(script: 'git fetch --tags --force && git describe --tags --abbrev=0', returnStdout: true).trim()
                      return [latestTag] ?: ["No tags found"]  // Return the latest tag or a fallback
                  })
    }
    stages {
        stage('Checkout SCM') {
            steps {
                script {
                    // Ensure all tags are fetched
                    sh 'git fetch --tags --force'
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    // Use the dynamically populated parameter
                    echo "Building with the latest Git tag: ${params.LATEST_TAG}"
                }
            }
        }
    }
}

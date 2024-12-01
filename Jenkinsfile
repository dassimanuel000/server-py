pipeline {
    agent any
    parameters {
        // Define a string parameter for the latest tag
        string(name: 'LATEST_TAG', defaultValue: '', description: 'The latest Git tag')
    }
    stages {
        stage('Get Latest Tag') {
            steps {
                script {
                    // Run the git command to get the latest tag and save it to a file
                    sh 'git describe --tags --abbrev=0 > latest_tag.txt'
                    
                    // Read the latest tag from the file
                    def latestTag = readFile('latest_tag.txt').trim()
                    echo "Latest Git Tag: ${latestTag}"
                    
                    // Set the LATEST_TAG parameter dynamically
                    currentBuild.description = "Latest Tag: ${latestTag}"
                    
                    // Set the parameter value using the input
                    env.LATEST_TAG = latestTag
                }
            }
        }
        
        stage('Build') {
            steps {
                script {
                    // Use the LATEST_TAG value in your build process
                    echo "Using the latest Git tag: ${LATEST_TAG}"
                    // Your build steps go here, you can pass the LATEST_TAG to a build tool or process
                }
            }
        }
    }
}

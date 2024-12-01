pipeline {
    agent any
    environment {
        // Define the environment variable to store the latest tag
        LATEST_TAG = ''
    }
    stages {
        stage('Get Latest Tag') {
            steps {
                script {
                    // Run the git command to get the latest tag and save it to a file
                    sh 'git describe --tags --abbrev=0 > latest_tag.txt'
                    
                    // Read the latest tag from the file and store it in the environment variable
                    LATEST_TAG = readFile('latest_tag.txt').trim()
                    echo "Latest Git Tag: ${LATEST_TAG}"
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

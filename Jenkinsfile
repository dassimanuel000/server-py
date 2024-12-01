pipeline {
    agent any
    parameters {
        string(name: 'LATEST_TAG', defaultValue: '', description: 'Latest Git tag') // Parameter for the latest tag
    }
    stages {
        stage('Checkout SCM') {
            steps {
                script {
                    // Ensure that all tags are fetched from the remote repository
                    sh 'git fetch --tags --force'
                }
            }
        }
        stage('Get Latest Tag') {
            steps {
                script {
                    // Run the git command to get the latest tag and store it in a file
                    sh 'git describe --tags --abbrev=0 > latest_tag.txt'
                    
                    // Read the latest tag from the file
                    def latestTag = readFile('latest_tag.txt').trim()
                    
                    // If no tags found, use a fallback value
                    if (!latestTag) {
                        latestTag = "no-tags"
                    }
                    
                    // Print the latest tag
                    echo "Latest Git Tag: ${latestTag}"
                    
                    // Set the LATEST_TAG parameter dynamically for the build (not actually modifying Jenkins parameter, but using it in the build)
                    currentBuild.description = "Build with tag: ${latestTag}"  // Display in build description
                    env.LATEST_TAG = latestTag  // Use in the pipeline steps
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    // You can use the LATEST_TAG value in your build process
                    echo "Using the latest Git tag: ${env.LATEST_TAG}"
                }
            }
        }
    }
}

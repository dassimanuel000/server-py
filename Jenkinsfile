pipeline {
    agent any
    parameters {
        // Defining the Active Choice Parameter to retrieve the latest Git tag
        activeChoiceParam('LATEST_TAG') {
            description('Select the latest Git Tag')
            choiceType('PT_SINGLE_SELECT')  // You can choose SINGLE_SELECT or MULTI_SELECT
            groovyScript {
                script("""
                    // Fetch the latest tag dynamically
                    def gitTag = sh(script: 'git fetch --tags && git describe --tags --abbrev=0', returnStdout: true).trim()

                    // If no tag is found, return a fallback message
                    if (gitTag) {
                        return [gitTag]
                    } else {
                        return ['No tags found']
                    }
                """)
                fallbackScript('return ["No tags found"]')  // Fallback in case of failure
            }
        }
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        
        stage('Use Latest Git Tag') {
            steps {
                script {
                    // Output the selected Git tag value from the parameter
                    echo "Selected Git tag: ${params.LATEST_TAG}"
                }
            }
        }
        
        stage('Build') {
            steps {
                echo "Build process using tag: ${params.LATEST_TAG}"
                // You can use the value of LATEST_TAG in the build stage
            }
        }
    }
}

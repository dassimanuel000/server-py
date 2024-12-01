pipeline {
    agent any
    parameters {
        activeChoiceParam('LATEST_TAG') {
            description('Select the latest Git tag')
            choiceType('PT_SINGLE_SELECT')
            groovyScript {
                script('''
                    // Fetch the latest tags from Git repository
                    def tag = sh(script: "git fetch --tags --force && git describe --tags --abbrev=0", returnStdout: true).trim()

                    // If no tag found, set a default value or error message
                    if (tag == '') {
                        return ["No tags found"]
                    } else {
                        return [tag]
                    }
                ''')
                fallbackScript('return ["Error fetching tags"]')
            }
        }
    }
    stages {
        stage('Get Latest Tag') {
            steps {
                script {
                    // Print out the selected tag in the Jenkins logs
                    echo "Selected Git tag: ${params.LATEST_TAG}"
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    // Use the selected tag for your build or deployment
                    echo "Building with tag: ${params.LATEST_TAG}"
                    // You can integrate the ${params.LATEST_TAG} into your build process here
                }
            }
        }
    }
}

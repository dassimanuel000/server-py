pipeline {
    agent any
    parameters {
        // Define the choice parameter to dynamically fetch the latest Git tag
        activeChoiceParam('LATEST_TAG') {
            description('Select the latest Git tag')
            choiceType('PT_SINGLE_SELECT')
            groovyScript {
                script('''
                    // Fetch the latest Git tag dynamically
                    def tag = sh(script: "git fetch --tags --force && git describe --tags --abbrev=0", returnStdout: true).trim()

                    // If no tag is found, return a default value or error message
                    if (tag == '') {
                        return ['No tags found']
                    } else {
                        return [tag]
                    }
                ''')
                fallbackScript('return ["No tags found"]')
            }
        }
    }
    stages {
        stage('Build') {
            steps {
                echo "Using the selected Git tag: ${params.LATEST_TAG}"
                // You can now use ${params.LATEST_TAG} in your build process
            }
        }
    }
}

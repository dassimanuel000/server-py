pipeline {
    agent any

    parameters {
        activeChoiceParam('LATEST_TAG') {
            description('Select the latest tag from the repository')
            choiceType('SINGLE_SELECT') // Dropdown style
            groovyScript {
                script("""
                    try {
                        def process = "git describe --tags --abbrev=0".execute(null, new File("${WORKSPACE}"))
                        def latestTag = process.text.trim()
                        return [latestTag]
                    } catch (Exception e) {
                        return ["unknown"] // Default fallback in case of error
                    }
                """)
                fallbackScript('"unknown"') // Fallback if script execution fails
            }
        }
    }

    stages {
        stage('Use Tag') {
            steps {
                script {
                    echo "Selected Tag: ${params.LATEST_TAG}"
                }
            }
        }
    }
}

pipeline {
    agent none

    parameters {
        activeChoiceParam('RELEASE_NUMBER') {
            description('Select the release number from the latest tag.')
            choiceType('SINGLE_SELECT') // Dropdown menu
            groovyScript {
                script("""
                    try {
                        def process = ['bash', '-c', 'git describe --tags --abbrev=0'].execute()
                        process.waitFor()
                        if (process.exitValue() == 0) {
                            return [process.text.trim()]
                        } else {
                            return ["unknown"]
                        }
                    } catch (Exception e) {
                        return ["unknown"]
                    }
                """)
                fallbackScript('"unknown"') // Value when the script fails
            }
        }
    }

    stages {
        stage('Display Tag') {
            agent any
            steps {
                echo "Selected Release Number: ${params.RELEASE_NUMBER}"
            }
        }
    }
}

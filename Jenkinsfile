pipeline {
    agent none

    parameters {
        activeChoiceParam('RELEASE_NUMBER') {
            description('This number corresponds to the release number N (BETA.N / RC.N)')
            choiceType('SINGLE_SELECT') // Dropdown selection
            groovyScript {
                script("""
                    try {
                        // Execute Git command to get the latest tag
                        def latestTag = "git describe --tags --abbrev=0".execute().text.trim()
                        return [latestTag]
                    } catch (Exception e) {
                        // Default value if the script fails
                        return ["unknown"]
                    }
                """)
                fallbackScript('"unknown"') // Fallback value
            }
        }
    }

    stages {
        stage('Example') {
            agent any
            options {
                timeout(time: 1, unit: 'SECONDS') // Timeout for the stage
            }
            steps {
                script {
                    // Print the chosen RELEASE_NUMBER parameter
                    echo "Selected Release Number: ${params.RELEASE_NUMBER}"
                }
            }
        }
    }
}

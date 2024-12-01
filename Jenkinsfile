properties([ 
    parameters([
        [
            $class: 'ChoiceParameter', 
            choiceType: 'PT_SINGLE_SELECT', 
            description: 'Select the latest tag from the repository', 
            filterLength: 1, 
            filterable: true, 
            name: 'LATEST_TAG', 
            randomName: 'choice-parameter-1234567890', 
            script: [
                $class: 'GroovyScript', 
                fallbackScript: [
                    classpath: [], 
                    sandbox: false, 
                    script: 'return ["No tags found"]' // Fallback if the script fails
                ], 
                script: [
                    classpath: [], 
                    sandbox: false, 
                    script: '''
                        try {
                            // Execute the Git command to get the latest tag
                            def process = "git describe --tags --abbrev=0".execute(null, new File("${WORKSPACE}"))
                            def latestTag = process.text.trim()
                            return [latestTag] // Return the tag in a list
                        } catch (Exception e) {
                            return ["Error fetching tag"] // Error fallback
                        }
                    '''
                ]
            ]
        ]
    ])
])

pipeline {
    agent any
    stages {
        stage('Validate') {
            steps {
                echo "Selected tag: ${params.LATEST_TAG}"
            }
        }
    }
}

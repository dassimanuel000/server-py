properties([
    parameters([
        [$class: 'CascadeChoiceParameter',
         choiceType: 'PT_SINGLE_SELECT',
         description: 'Select the latest tag',
         name: 'RELEASE_NUMBER',
         script: [$class: 'GroovyScript',
                  script: [script: '''
                      try {
                          def latestTag = "git describe --tags --abbrev=0".execute().text.trim()
                          return [latestTag]
                      } catch (Exception e) {
                          return ["unknown"]
                      }
                  ''',
                           fallbackScript: 'return ["unknown"]']]
        ]
    ])
])

pipeline {
    agent any

    stages {
        stage('Verify Parameters') {
            steps {
                echo "Selected Release Number: ${params.RELEASE_NUMBER}"
            }
        }
    }
}

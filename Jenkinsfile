properties([
   parameters([
      [
         $class: 'ChoiceParameter',
         choiceType: 'PT_SINGLE_SELECT',
         description: 'Select the latest Git tag',
         filterLength: 1,
         filterable: true,
         name: 'LATEST_TAG',
         script: [
            $class: 'GroovyScript',
            fallbackScript: [
               classpath: [],
               sandbox: true,
               script: "return ['No tags found']" // Fallback if no tags are found
            ],
            script: [
               classpath: [],
               sandbox: true,
               script: """
                  // Fetch the latest tag using git commands
                  def gitTag = sh(script: 'git fetch --tags && git describe --tags --abbrev=0', returnStdout: true).trim()

                  // If a tag is found, return it; otherwise return a default message
                  if (gitTag) {
                      return [gitTag]
                  } else {
                      return ['No tags found']
                  }
               """
            ]
         ]
      ]
   ])
])

pipeline {
    agent any
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
                // You can use the value of LATEST_TAG in your build steps here
            }
        }
    }
}

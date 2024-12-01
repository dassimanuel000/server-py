pipeline {
    agent none

    environment {
        LATEST_TAG = sh(script: 'git describe --tags --abbrev=0', returnStdout: true).trim()
    }

    parameters {
        choice(name: 'RELEASE_NUMBER', 
               choices: "${LATEST_TAG},unknown".split(','), 
               description: 'Select the release number')
    }

    stages {
        stage('Display') {
            agent any
            steps {
                echo "Selected tag: ${params.RELEASE_NUMBER}"
            }
        }
    }
}

pipeline {
    agent any
    stages {
        stage('Find Previous Tag') {
            steps {
                script {
                    sh 'git fetch --tags --force'

                    def currentTag = '1.2.0-BETA.12' // Exemple : tag actuel
                    def branch = 'master' // Exemple : branche de la série de tags

                    def previousTag = findPreviousTag(currentTag, branch)

                    if (previousTag) {
                        echo "Current Tag: ${currentTag}"
                        echo "Previous Tag: ${previousTag}"
                    } else {
                        echo "No previous tag found for ${currentTag} on branch ${branch}."
                    }
                }
            }
        }
    }
}

def findPreviousTag(String currentTag, String branch) {
    def previousTag = sh(
        script: """
        tags=\$(git tag --merged ${branch} --sort=v:refname)
        previous=""
        
        for tag in \$tags; do
            if [[ \$tag =~ ^1\.[0-9]+\.[0-9]+-(RC|BETA)\.[0-9]+$ ]]; then
                if [[ \$tag == "${currentTag}" ]]; then
                    break
                fi
                previous=\$tag
            fi
        done
        
        echo \$previous
        """,
        returnStdout: true
    ).trim()

    if (previousTag) {
        echo "Found previous tag: ${previousTag}"
    } else {
        echo "No previous tag found before ${currentTag}."
    }

    return previousTag ? previousTag : null
}

pipeline {
    agent any
    stages {
        stage('Find Previous Tag') {
            steps {
                script {
                    // Force fetch all tags from the repository
                    sh 'git fetch --tags --force'


                    def currentTag = '1.2.0-BETA.12' // Exemple : tag actuel
                    def branch = 'master' // Exemple : branche de la série de tags


                    def previousTag = findPreviousTag(currentTag, branch)

                    // Afficher le résultat
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
    // Exécution de la commande Bash pour trouver tous les tags
    def previousTag = sh(
        script: """
        tags=\$(git tag --merged ${branch} --sort=v:refname)

        previous=""
        
        for tag in \$tags; do
            if [[ "\$tag" =~ ^1\.[0-9]+\.[0-9]+-(RC|BETA)\.[0-9]+$ ]]; then
                # Si on trouve le tag actuel, on arrête
                if [[ "\$tag" == "${currentTag}" ]]; then
                    break
                fi
                previous=\$tag
            fi
        done
        
        # Retourner le précédent tag trouvé
        echo \$previous
        """,
        returnStdout: true
    ).trim()

    // Retourner le tag trouvé ou null s'il n'y en a pas
    return previousTag ? previousTag : null
}

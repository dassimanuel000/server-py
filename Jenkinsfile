pipeline {
    agent any
    stages {
        stage('Find Previous Tag') {
            steps {
                script {
                    // Force fetch all tags from the repository
                    sh 'git fetch --tags --force'

                    // Tag actuel fourni (par exemple, via une variable GitLab ou hardcodé ici)
                    def currentTag = '1.1.0-RC.5' // Exemple : tag actuel
                    def branch = 'release-1.1.0' // Exemple : branche de la série de tags

                    // Appel de la fonction pour trouver le précédent tag
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
    // Exécution de la commande Bash pour trouver le précédent tag
    def previousTag = sh(
        script: """
        # Lister les tags triés par version uniquement sur la branche donnée
        tags=\$(git tag --merged ${branch} --sort=v:refname)
        previous=""
        
        # Parcourir les tags pour trouver le précédent
        for tag in \$tags; do
            if [[ "\$tag" == "${currentTag}" ]]; then
                break
            fi
            previous=\$tag
        done
        
        # Retourner le précédent tag trouvé
        echo \$previous
        """,
        returnStdout: true
    ).trim()

    // Retourner le tag trouvé ou null s'il n'y en a pas
    return previousTag ? previousTag : null
}

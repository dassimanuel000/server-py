pipeline {
    agent any
    stages {
        stage('Git Init and Checkout Master') {
            steps {
                script {
                    // Vérifie si le dépôt est déjà initialisé
                    sh '''
                    if [ ! -d ".git" ]; then
                        echo "Initialisation du dépôt Git"
                        git init
                        git config user.name "Jenkins"
                        git config user.email "jenkins@localhost"
                    fi
                    '''
                    
                    // Assurer que la branche 'master' existe, sinon créer et y basculer
                    sh '''
                    git fetch --all
                    if ! git show-ref --verify --quiet refs/heads/master; then
                        echo "Branche master inexistante, création de master..."
                        git checkout -b master
                        git commit --allow-empty -m "Initial commit"
                    else
                        echo "Branche master existante, basculement vers master..."
                        git checkout master
                    fi
                    '''

                    // Vérifier l'état du dépôt et afficher la branche actuelle
                    sh '''
                    git status
                    '''
                }
            }
        }

        stage('Find Previous Tag') {
            steps {
                script {
                    def currentTag = '1.2.0-BETA.12' // Exemple : tag actuel
                    def branch = 'master' // Branche sur laquelle on cherche les tags

                    // Fonction pour trouver le tag précédent
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

// Fonction pour trouver le tag précédent
def findPreviousTag(String currentTag, String branch) {
    def previousTag = sh(
        script: """
        git fetch --tags --force
        tags=\$(git tag --merged ${branch} --sort=v:refname)
        previous=""

        for tag in \$tags; do
            if echo "\$tag" | grep -E '^1\\.[0-9]+\\.[0-9]+-(RC|BETA)\\.[0-9]+\$' > /dev/null; then
                if [ "\$tag" = "${currentTag}" ]; then
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

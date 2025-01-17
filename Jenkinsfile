pipeline {
    agent any
    stages {
        stage('Git Init and Checkout Master') {
            steps {
                script {
                    // Vérifie si le dépôt est initialisé et configure l'utilisateur si nécessaire
                    sh '''
                    if [ ! -d ".git" ]; then
                        git init
                    fi

                    git config user.name "Jenkins"
                    git config user.email "jenkins@localhost"

                    git fetch --all
                    if ! git show-ref --verify --quiet refs/heads/master; then
                        git checkout -b master
                        git commit --allow-empty -m "Initial commit"
                    else
                        git checkout master
                    fi
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

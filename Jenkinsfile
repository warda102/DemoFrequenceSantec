pipeline {
    agent any  // Utiliser n'importe quel agent disponible

    triggers {
        // Déclenche la pipeline lors de push sur dev1 ou dev2
        githubPush()
    }

    environment {
        GIT_BRANCH_DEV = 'dev'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Merge to dev') {
            steps {
                script {
                    // Utilisation de GIT_BRANCH pour obtenir le nom de la branche
                    def branchName = env.GIT_BRANCH.replaceAll('origin/', '')

                    echo "Branch Name: ${branchName}"

                    // Vérifier si la branche actuelle est dev1 ou dev2
                    if (branchName == 'dev1' || branchName == 'dev2') {
                        echo "Merging ${branchName} into dev"

                        sh """
                            git checkout dev
                            git pull origin dev
                            git merge ${branchName} || { echo 'Merge conflict detected!'; exit 1; }
                        """

                        // Vérifier s'il y a des changements à pousser
                        def changes = sh(script: "git status --porcelain", returnStdout: true).trim()
                        if (changes) {
                            echo "Changes detected, pushing to dev..."
                            sh "git push origin dev"
                        } else {
                            echo "No changes to push."
                        }
                    } else {
                        echo "No merge needed for this branch: ${branchName}"
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()  // Nettoyer l'espace de travail
        }
        success {
            echo "Merge completed successfully!"
        }
        failure {
            echo "Merge failed!"
        }
    }
}

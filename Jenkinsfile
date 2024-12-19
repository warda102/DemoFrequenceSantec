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
        stage('Merge to dev') {
            steps {
                script {
                    // Utiliser rev-parse pour obtenir la branche courante, même en état détaché
                    def currentBranch = sh(script: 'git rev-parse --abbrev-ref HEAD', returnStdout: true).trim()
                    echo "Current Branch: ${currentBranch}"

                    if (currentBranch == 'dev1') {
                        echo "Merging dev1 into dev"
                        sh 'git checkout dev'  // Passe à la branche dev
                        sh 'git merge dev1'    // Fusionne dev1 dans dev

                        // Vérifie si le merge a été effectué correctement
                        sh 'git status'
                        sh 'git log --oneline --graph'

                        // Push les changements sur GitHub
                        sh 'git push origin dev'
                    } else {
                        echo "No merge needed for this branch: ${currentBranch}"
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

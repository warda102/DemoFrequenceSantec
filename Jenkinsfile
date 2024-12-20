pipeline {
    agent any
    triggers {
        githubPush()  // Le pipeline se déclenche lorsqu'un push est effectué sur GitHub
    }
    stages {
        stage('Checkout') {
            steps {
                // Vérification de la branche actuelle à partir du dépôt Git
                checkout scm
                script {
                    echo "Current Branch: ${env.BRANCH_NAME}"
                }
            }
        }
        stage('Merge and Push') {
            steps {
                script {
                    // Vérification si la branche est dev1 ou dev2
                    if (env.BRANCH_NAME == 'dev1' || env.BRANCH_NAME == 'dev2') {
                        echo "Merging ${env.BRANCH_NAME} into dev"

                        // Récupérer toutes les branches depuis le dépôt distant
                        sh 'git fetch origin'

                        // S'assurer d'être sur la branche dev
                        sh 'git checkout dev'

                        // Mettre à jour la branche dev avec le dépôt distant
                        sh 'git pull origin dev'

                        // Fusionner dev1 ou dev2 dans dev
                        sh "git merge origin/${env.BRANCH_NAME}"

                        // Pousser les modifications sur la branche dev
                        sh 'git push origin dev'
                    } else {
                        echo "Not on dev1 or dev2. Skipping merge to dev."
                    }
                }
            }
        }
    }
    post {
        success {
            echo 'Merge completed successfully!'
        }
        failure {
            echo 'Merge failed!'
        }
    }
}

pipeline {
    agent any

    triggers {
        githubPush()
    }

    environment {
        GIT_BRANCH_DEV = 'dev'
    }

    stages {
        stage('Merge to dev') {
            steps {
                script {
                    // Identifier la branche courante à partir de Jenkins
                    def currentBranch = env.GIT_BRANCH.replaceFirst('origin/', '')
                    echo "Current Branch: ${currentBranch}"

                    if (currentBranch == 'dev1' || currentBranch == 'dev2') {
                        echo "Merging ${currentBranch} into dev"

                        // Mettre à jour la branche dev
                        sh 'git checkout dev'
                        sh 'git pull origin dev'

                        // Fusionner la branche courante dans dev
                        sh "git merge ${currentBranch}"

                        // Pousser les changements
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
            cleanWs()
        }
        success {
            echo "Merge completed successfully!"
        }
        failure {
            echo "Merge failed!"
        }
    }
}

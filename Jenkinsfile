pipeline {
    agent any
    triggers {
        githubPush()
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
        stage('Merge to dev') {
            steps {
                script {
                    // Vérifier si on est sur dev1 ou dev2
                    if (env.BRANCH_NAME == 'dev1' || env.BRANCH_NAME == 'dev2') {
                        // Récupérer toutes les branches et se positionner sur dev
                        sh 'git fetch origin'
                        sh 'git checkout dev'
                        
                        // Fusionner la branche courante (dev1 ou dev2) dans dev
                        sh "git merge origin/${env.BRANCH_NAME}"

                        // Push vers la branche dev
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

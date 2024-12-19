pipeline {
    agent any
    triggers {
        // Déclenchement sur les push sur dev1 et dev2
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
                    if (env.BRANCH_NAME == 'dev1' || env.BRANCH_NAME == 'dev2') {
                        // Assurez-vous de bien faire un fetch pour toutes les branches
                        sh 'git fetch origin'

                        // Checkout de la branche dev
                        sh 'git checkout dev'

                        // Fusion de la branche dev1 ou dev2 dans dev
                        sh 'git merge origin/' + env.BRANCH_NAME

                        // Push des changements sur dev
                        sh 'git push origin dev'
                    }
                }
            }
        }
    }
}

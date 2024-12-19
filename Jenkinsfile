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
                        sh 'git checkout dev'
                        sh 'git merge ' + env.BRANCH_NAME
                        sh 'git push origin dev'
                    }
                }
            }
        }
    }
}

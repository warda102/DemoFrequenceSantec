pipeline {
    agent any

    environment {
        GITHUB_CREDENTIALS = 'GitHub-Token'
        REPO_URL = 'https://github.com/warda102/DemoFrequenceSantec.git'
    }

    triggers {
        githubPush()
    }

    stages {
        stage('Checkout Source Branch') {
            steps {
                script {
                    echo "Cloning the source branch (e.g., dev1)..."
                }
                checkout([$class: 'GitSCM', 
                          branches: [[name: '*/dev1']], // Branche source
                          userRemoteConfigs: [[
                              url: env.REPO_URL,
                              credentialsId: env.GITHUB_CREDENTIALS
                          ]]
                ])
            }
        }

        stage('Merge with Target Branch') {
            steps {
                script {
                    echo "Merging the source branch into dev..."
                    sh """
                        git config user.name "jenkins-bot"
                        git config user.email "jenkins@localhost"

                        # Récupérer et configurer les branches
                        git fetch origin dev:dev
                        git checkout dev
                        git fetch origin dev1:dev1

                        # Fusionner les branches et gérer les conflits
                        git merge dev1 --no-edit || true
                        git checkout --theirs -- Exemple.html || echo 'Conflit non résolu automatiquement'
                        git checkout --theirs -- Jenkinsfile || echo 'Conflit non résolu automatiquement'
                        git add Exemple.html Jenkinsfile
                        git commit -m 'Résolution automatique des conflits'
                    """
                }
            }
        }

        stage('Push to Target Branch') {
            steps {
                script {
                    echo "Pushing the merged changes to dev..."
                    sh """
                        git push origin dev
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Merge and push to dev completed successfully!"
        }
        failure {
            echo "Pipeline failed. Check the logs for details."
        }
    }
}

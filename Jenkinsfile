pipeline {
    agent any

    environment {
        // Nom de l'identifiant des credentials configurés dans Jenkins
        GITHUB_CREDENTIALS = 'GitHub-Token' 
        // URL de votre dépôt GitHub
        REPO_URL = 'https://github.com/warda102/DemoFrequenceSantec.git'
    }

    stages {
        stage('Checkout Source Branch') {
            steps {
                script {
                    echo "Cloning the source branch (e.g., dev1)..."
                }
                checkout([$class: 'GitSCM', 
                          branches: [[name: '*/dev1']], // Branche source (par exemple dev1)
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
                    // Configure git pour utiliser le jeton d'accès
                    sh """
                        git config user.name "jenkins-bot"
                        git config user.email "jenkins@localhost"
                        git fetch origin dev
                        git checkout dev
                        git merge dev1 --no-edit
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

stage('Merge to dev') {
    steps {
        script {
            // Vérifier si la branche actuelle est dev1 ou dev2
            def branchName = env.BRANCH_NAME
            echo "Branch name: ${branchName}"

            if (branchName == 'dev1' || branchName == 'dev2') {
                echo "Merging ${branchName} into dev"
                sh """
                    # Se déplacer sur la branche dev
                    git checkout dev

                    # Récupérer les dernières modifications de dev
                    git pull origin dev

                    # Effectuer le merge de dev1 ou dev2 vers dev
                    git merge ${branchName}

                    # Pousser les modifications sur la branche dev
                    git push origin dev
                """
            } else {
                echo "No merge needed for this branch: ${branchName}"
            }
        }
    }
}

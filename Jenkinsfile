pipeline {
    agent any

    environment {
        APP_NAME = "register-app-pipeline"
        IMAGE_TAG = "prathap" // Replace with dynamic tag if needed
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/prathapKitti/gitops-register-app-1'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                    echo "Before:"
                    cat deployment.yaml

                    sed -i 's|${APP_NAME}:.*|${APP_NAME}:${IMAGE_TAG}|g' deployment.yaml

                    echo "After:"
                    cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github-pat-prathap', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_TOKEN')]) {
                    sh '''
                        git config --global user.name "$GIT_USER"
                        git config --global user.email "prathapakindia@gmail.com"
                        git remote set-url origin https://$GIT_USER:$GIT_TOKEN@github.com/prathapKitti/gitops-register-app-1.git

                        if git diff --quiet; then
                            echo "✅ No changes to commit."
                        else
                            git add deployment.yaml
                            git commit -m "Updated Deployment Manifest"
                            git push origin main
                        fi
                    '''
                }
            }
        }
    }
}

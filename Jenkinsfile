pipeline {
    agent any
    environment {
              APP_NAME = "register-app-pipeline"
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
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "prathapKitti"
                   git config --global user.email "prathapakindia@gmail.com"
                   git config --global credential.https://github.com/prathapKitti/gitops-register-app-1.prathapKitti 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
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                   git push https://prathapKitti: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@github.com/prathapKitti/gitops-register-app-1 main
                """
                // withCredentials([string(credentialsId: 'token1', variable: 'token1', gitToolName: 'Default')]){
                //   sh "git push https://github.com/prathapKitti/gitops-register-app-1 main"
                // }
            }
        }
      
    }
}

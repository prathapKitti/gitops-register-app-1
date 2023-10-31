pipeline {
    agent { label "Jenkins-Agent" }
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
                   git branch: 'main', credentialsId: 'github', url: 'https://github.com/sagarkulkarni1989/gitops-register-app'
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
                   git config --global user.name "sagarkulkarni1989"
                   git config --global user.email "mesagarkulkarni@gmail.com"
                   git config --global credential.https://github.com/sagarkulkarni1989/gitops-register-app.sagarkulkarni1989 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
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                   git push https://sagarkulkarni1989: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@github.com/sagarkulkarni1989/gitops-register-app main
                """
                // withCredentials([string(credentialsId: 'token1', variable: 'token1', gitToolName: 'Default')]){
                //   sh "git push https://github.com/sagarkulkarni1989/gitops-register-app main"
                // }
            }
        }
      
    }
}

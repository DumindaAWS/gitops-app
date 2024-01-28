pipeline {
    agent any
    environment {
              APP_NAME = "shopping-cart4"
              IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {
       
        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: '9deec83f-f312-44a0-aa36-226885ab73f1', url: 'https://github.com/DumindaAWS/gitops-app.git'
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
                   git config --global user.name "DumindaAWS"
                   git config --global user.email "aws.duminda@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: '9deec83f-f312-44a0-aa36-226885ab73f1', gitToolName: 'Default')]) {
                  sh "git push https://github.com/DumindaAWS/gitops-app main"
                }
            }
        }
      
    }
}

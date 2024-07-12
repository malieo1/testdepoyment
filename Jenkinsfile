pipeline {
  agent any
  environment {
    APP_NAME ="cicd-stage"
  }

  stages {
    stage ("cleanup workplace"){
        steps{
            clearWs()
        }
    }

    stage("chekout from scm"){
        steps{
            git branch: 'main', credentialsId: 'github', url: 'https://github.com/malieo1/testdepoyment.git'
        }
    }

    stage("update the deployment tags"){
        steps{
            sh """
                cat front-deployment.yaml
                sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' front-deployment.yaml
                cat front-deployment.yaml 
            """
        }
    }

    stage("Push the change"){
        steps{
            sh """
                git config --global user.name "malieo1"
                git config --global user.email "malek.zahmoul@esprit.tn"
                git add front-deployment.yaml
                git commit -m "updated deployment"
            """
            withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]){
                sh "git push https://github.com/malieo1/testdepoyment.git main"
            }
        }
    }

  }
}
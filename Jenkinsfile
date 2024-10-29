pipeline {
    agent any
    environment {
        APP_NAME = "e2e-devops-jenkins"
    }
    stages{
        stage("Cleanup Workspace"){
            steps{
                cleanWs()
            }
        }
        stage("Checkout from SCM"){
            steps{
                git(branch: 'main', credentialsId: 'github', url: 'https://github.com/soufiane-devops-projects/gitops-complete-production-e2e-pipeline')
            }
        } 
        stage("Update the Deployment Tags"){
            steps{
                sh """
                    cat deployment.yml
                    sed -i "s/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g" deployment.yml
                    cat deployment.yml
                """
            }
        }
        stage("Push the changed deployment to Git"){
            steps{
               sh '''
                    git config --global user.name "sadevops"
                    git config --global user.email "sadev@gmail.com"
                    git add deployment.yml
                    git commit -m "Updated deployment manifest"
               '''
               withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'default')]){
                    sh('git push https://github.com/soufiane-devops-projects/gitops-complete-production-e2e-pipeline main')
               }
            }
        }
    }

}
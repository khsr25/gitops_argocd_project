pipeline{

    agent any 

    environment{
        DOCKERHUB_USERNAME = "khsr25"
        APP_NAME ="gitops-argo-app"
        IMAGE_TAG ="${BUILD_NUMBER}"
        IMAGE_NAME ="${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
        REGISTRY_CREDS = 'dockerhub'
    }

    stages{

        stage('Clean Workspace'){

            steps{
                script{

                    cleanWs()
                }
            }
        }
        stage('Checkout SCM'){
            steps{
                git credentialsId: 'github',
                url: 'https://github.com/khsr25/gitops_argocd_project.git',
                branch: 'main'
            }
        }
    }
}
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
        stage('Build Docker image'){
            steps{
                script{

                    docker_image = docker.build "${IMAGE_NAME}"
                }
            }
        }
        stage('Push Docker Image'){
            steps{
                script{

                    docker.withRegistry('',REGISTRY_CREDS){
                        docker_image.push("$BUILD_NUMBER")
                        docker_image.push('latest')
                    }
                }
            }
        }
        stage('Delete Docker images'){

            steps{
                script{

                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
                }
            }
        }
        stage('Updating k8s Deployment file'){
            steps{
                script{

                    sh """
                    cat deployment.yml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yml
                    cat deployment.yml
                    """
                }
            }
        }
        stage('push the changed Deployment file to Git'){
            steps{
                script{
                    sh """
                      git config --global user.name "khsr25"
                      git config --global user.email "khsr25@gmail.com"
                      git add deployment.yml
                      git commit -m "updated the deployment file"
                      """
                      withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]){

                      sh "git push https://github.com/khsr25/gitops_argocd_project.git main"  
                      }
                }
            }
        }
    }
}
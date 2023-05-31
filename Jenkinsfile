pipeline{

    agent any

    environment{     
        APP_NAME = "gitops_arogocd"
    }

    stages{

        stage('Cleanup Workspace'){
            
            steps{
                script{
                    cleanWs()
                }
            }
        }

        stage ('Checkout'){

            steps{
                script{
                    git credentialsId: 'GitHub',
                    url: 'https://github.com/chirag0002/GitOps_Argo_CD.git',
                    branch: 'main'
                }
            }
        }

        stage ('Update Kubernetes Deployment'){

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

        stage ('Push changed deployment to GitHub'){ 
            steps{
                script{
                    sh """
                      git config --global user.email "varshneychirag34@gmail.com"
                      git config --global user.name "chirag0002"
                      git add deployment.yml
                      git commit -m "Updated deployment"
                    """

                    withCredentials([gitUsernamePassword(credentialsId: 'GitHub', gitToolName: 'Default')]) {
                        sh "git push https://github.com/chirag0002/GitOps_Argo_CD.git main"
                    }  
                }
            }
        }
    }
}

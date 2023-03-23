pipeline{
    agent any
    
    environment{
        APP_NAME = "jenkinsCI_argoCD"
    }

    stages{
        stage('cleanup workspace'){
            steps{
                script{
                    cleanWs()
                }
            }
        }
        stage('Checkout SCM'){
            steps{
                script{
                    git credentialsId: 'github',
                    url: 'https://github.com/syscrafts/ArgoCD',
                    branch: 'main'
                }
            }
        }
        stage('Updating kubernetes deployment file'){
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
        stage('Push the changed deployment file to Git'){
            steps{
                script{
                    sh """
                        git config --global user.name "syscrafts"
                        git config --global user.email "whitecollarsamrat@gmail.com"
                        git add deployment.yml
                        git commit -m "updated the deployment file" 
                    """
                    withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolname: 'Default')]){
                        sh "git push origin main"}
                }
            }
        }
    }
}



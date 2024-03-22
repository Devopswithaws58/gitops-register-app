pipeline{
    agent{
        label 'Jenkins-Agent'
    }
    environment{
        APP_NAME = 'register-app'
    }
    stages{
        stage('clean workspace'){
            steps{
                cleanWs()
            }
        }
        stage('checkout from SCM'){
            steps{
                git branch: 'main',
                    credentialsId: 'github',
                    url: 'https://github.com/Devopswithaws58/gitops-register-app.git'
            }
        }
        stage('update the deployment tags'){
            steps{
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
            }
        }
        stage('push the changed deployment file to git'){
            steps{
                sh """
                    git config --global user.name "devopswthaws58"
                    git config --global user.email "devopswithaws58@gmail.com"
                    git add deployment.yaml
                    git commit -m 'updated deployment manifest file'
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]){
                    sh 'git push https://github.com/Devopswithaws58/gitops-register-app.git main'
                }
            }
        }
    }
}

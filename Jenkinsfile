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
                   git branch: 'main', credentialsId: 'github', url: 'https://github.com/fahadriaz1206/gitops-register-app'
               }
        }

        stage("Update the Deployment Tags") {
                steps {
                    sh """
                    echo "Before Update:"
                    cat deployment.yaml
                    sed -i 's|231791477922.dkr.ecr.ap-northeast-1.amazonaws.com/register-app-pipeline:.*|231791477922.dkr.ecr.ap-northeast-1.amazonaws.com/register-app-pipeline:${IMAGE_TAG}|g' deployment.yaml
                    echo "After Update:"
                    cat deployment.yaml
                    """
                }
        }


        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "fahadriaz1206"
                   git config --global user.email "fahadriaz1206@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                  sh "git push https://github.com/fahadriaz1206/gitops-register-app main"
                }
            }
        }
      
    }
}

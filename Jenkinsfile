pipeline{
    agent{
        label "jenkins-agent"
    }
    environment {
        APP_NAME = "bootapp-e2e-pipeline-demo"
        GIT_USER_NAME = "RT-MANO"
        GIT_REPO_NAME = "deploy-manifests"

    }
    stages{
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }

        }
        stage("Checkout from SCM"){
            steps {
                git branch: 'master', credentialsId: 'github', url: "https://github.com/${GIT_USER_NAME}/${GIT_REPO_NAME}"
            }

        }
        stage("Update Deployment Tag") {
            steps {
                sh """
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                """
            }

        }
        stage("Push the updated Deployment file to Repo") {
            steps {
                sh """
                    git config --global user.name "RT-MANO"
                    git config --global user.email "rt_mano@yahoo.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest!..."
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} HEAD:master"
                }
            }
        }

    }

}

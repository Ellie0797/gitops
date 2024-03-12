pipeline {
    agent {
        label "jenkins-agent"
    }

    environment {
        APP_NAME = "complete-prodcution-e2e-pipeline"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
        }
    }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Ellie0797/gitops'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.* /${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
            }
        }


        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                    git config --global user.nane "ellie0797"
                    git config --global user.email "elliegwon7@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """

                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/Ellie0797/gitops main"
                }
            }
        }
    }
}
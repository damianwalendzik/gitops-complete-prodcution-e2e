pipeline {
    agent {
        label "jenkins-agent"
    }
    environment {
        APP_NAME = "complete-production-e2e-pipeline"
        JENKINS_API_TOKEN = 
    }
    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/damianwalendzik/gitops-complete-production-e2e'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh '''
                cat deployment.yaml
                sed -i s/${APP_NAME}:.*$/${APP_NAME}:${IMAGE_TAG}/g deployment.yaml
                cat deployment.yaml
                '''
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh '''
                git config --global user.name "damianwalendzik"
                git config --global user.email "damian.cloud"
                git add deployment.yaml
                git commit -m "Updated Deployment Manifest"
                '''
                withCredentials([usernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/damianwalendzik/gitops-complete-production-e2e main"
                }
            }
        }
    }
}

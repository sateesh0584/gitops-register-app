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
                git branch: 'main', credentialsId: 'GitHub', url: 'https://github.com/sateesh0584/register-app'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                script {
                    def deploymentFile = "deployment.yaml"
                    if (fileExists(deploymentFile)) {
                        sh "cat ${deploymentFile}"
                        sh "sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' ${deploymentFile}"
                        sh "cat ${deploymentFile}"
                    } else {
                        echo "Deployment file (${deploymentFile}) not found."
                    }
                }
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                script {
                    def deploymentFile = "deployment.yaml"
                    if (fileExists(deploymentFile)) {
                        sh """
                           git config --global user.name "sateesh0584"
                           git config --global user.email "sateesh0584@gmail.com"
                           git add ${deploymentFile}
                           git commit -m "Updated Deployment Manifest"
                           git push https://github.com/sateesh0584/gitops-register-app main
                        """
                    } else {
                        echo "Skipping git push as deployment file (${deploymentFile}) not found."
                    }
                }
            }
        }
      
    }
}

def fileExists(filePath) {
    return file(filePath).exists()
}

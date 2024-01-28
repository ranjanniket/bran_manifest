pipeline {
    agent any

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
        GIT_REPO_NAME = "bran_manifest"
        GIT_USER_NAME = "ranjanniket"
        NEW_BUILD_NUMBER = "${BUILD_NUMBER}"
    }

    stages {
        stage('clean workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout from Git') {
            steps {
                git branch: 'main', url: 'https://github.com/ranjanniket/bran_manifest.git'
            }
        }

        stage('Update Deployment File') {
            steps {
                withCredentials([string(credentialsId: 'ranjanniket', variable: 'GITHUB_TOKEN')]) {
                    sh '''
                        git config user.email "niketranjn50@gmail.com"
                        git config user.name "Niket Ranjan"
                        sed -i "s/niket50\\/bran:.*/niket50\\/bran:${NEW_BUILD_NUMBER}/" Kubernetes/bran.yaml
                        git add .
                        git commit -m "Update image tag to ${NEW_BUILD_NUMBER}"
                        git push https://${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} HEAD:main
                    '''
                }
            }
        }
    }
}


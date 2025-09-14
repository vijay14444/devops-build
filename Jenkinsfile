pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('Shivangi@2023')   // Add creds in Jenkins
        DOCKER_HUB_USER = 'vijayguptacloud'              // Change this
        DEV_REPO = 'vijayguptacloud/dev'
        PROD_REPO = 'vijayguptacloud/prod'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: "${env.BRANCH_NAME}", url: 'https://github.com/vijay14444/devops-build.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh './build.sh'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    if (env.BRANCH_NAME == "dev") {
                        sh """
                        echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                        docker tag my-react-app:latest $DEV_REPO:latest
                        docker push $DEV_REPO:latest
                        """
                    } else if (env.BRANCH_NAME == "master") {
                        sh """
                        echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                        docker tag my-react-app:latest $PROD_REPO:latest
                        docker push $PROD_REPO:latest
                        """
                    }
                }
            }
        }

        stage('Deploy to Server') {
            steps {
                sh './deploy.sh'
            }
        }
    }
}

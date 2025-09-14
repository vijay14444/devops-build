pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                sh './build.sh'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    script {
                        if (env.BRANCH_NAME == "dev") {
                            sh """
                            echo $PASS | docker login -u $USER --password-stdin
                            docker tag my-react-app:latest $USER/dev:latest
                            docker push $USER/dev:latest
                            """
                        } else if (env.BRANCH_NAME == "master") {
                            sh """
                            echo $PASS | docker login -u $USER --password-stdin
                            docker tag my-react-app:latest $USER/prod:latest
                            docker push $USER/prod:latest
                            """
                        }
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                sh './deploy.sh'
            }
        }
    }
}

pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = 'deepu09567'
        IMAGE_NAME = 'deepu09567/deepu'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'

                git branch: 'main',
                    url: 'https://github.com/deepubhakuni5-create/deepu.git'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'

                bat '''
                    docker build -t %IMAGE_NAME%:latest .
                '''
            }
        }

        stage('Docker Login') {
            steps {
                echo 'Logging into Docker Hub...'

                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    bat '''
                        echo %DOCKER_PASSWORD% | docker login -u "%DOCKER_USER%" --password-stdin
                    '''
                }
            }
        }

        stage('Docker Push') {
            steps {
                echo 'Pushing image to Docker Hub...'

                bat '''
                    docker push %IMAGE_NAME%:latest
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                echo 'Deploying container on Windows machine...'

                bat '''
                    docker stop deepu 2>NUL || exit 0
                    docker rm deepu 2>NUL || exit 0

                    docker pull %IMAGE_NAME%:latest

                    docker run -d ^
                        --name deepu ^
                        -p 8090:80 ^
                        %IMAGE_NAME%:latest
                '''
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline completed successfully!'
            echo 'Website: http://localhost:8090'
        }

        failure {
            echo 'CI/CD Pipeline failed.'
        }
    }
}

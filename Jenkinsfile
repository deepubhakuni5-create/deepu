pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = 'deepu09567'
        IMAGE_NAME = 'deepu09567/staticside'
        IMAGE_TAG = 'latest'
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
                    docker build -t %IMAGE_NAME%:%IMAGE_TAG% .
                '''
            }
        }

        stage('Docker Login') {
            steps {
                echo 'Logging into Docker Hub...'

                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    bat '''
                        docker logout
                        echo %DOCKER_PASSWORD% | docker login --username "%DOCKER_USER%" --password-stdin
                    '''
                }
            }
        }

        stage('Docker Push') {
            steps {
                echo 'Pushing image to Docker Hub...'

                bat '''
                    docker push %IMAGE_NAME%:%IMAGE_TAG%
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                echo 'Deploying container on Windows machine...'

                bat '''
                    docker stop staticwebsite >NUL 2>&1
                    docker rm staticwebsite >NUL 2>&1

                    docker pull %IMAGE_NAME%:%IMAGE_TAG%

                    docker run -d ^
                        --name staticwebsite ^
                        -p 1748:80 ^
                        %IMAGE_NAME%:%IMAGE_TAG%
                '''
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline completed successfully!'
            echo 'Docker Image: deepu09567/staticside:latest'
            echo 'Container: staticwebsite'
            echo 'Website: http://localhost:1748'
        }

        failure {
            echo 'CI/CD Pipeline failed.'
        }
    }
}

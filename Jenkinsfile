pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = 'deepu09567'
        IMAGE_NAME = 'deepu09567/deepu'

        DOCKER = 'C:\\Users\\Ankit\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'

                git branch: 'main',
                    url: 'https://github.com/deepubhakuni5-create/deepu.git'
            }
        }

        stage('Docker Info') {
            steps {
                echo 'Checking Docker...'

                bat '''
                    whoami
                    "%DOCKER%" version
                    "%DOCKER%" info
                '''
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'

                bat '''
                    "%DOCKER%" build -t %IMAGE_NAME%:latest .
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
                        echo %DOCKER_PASSWORD% | "%DOCKER%" login -u "%DOCKER_USER%" --password-stdin
                    '''
                }
            }
        }

        stage('Docker Push') {
            steps {
                echo 'Pushing image to Docker Hub...'

                bat '''
                    "%DOCKER%" push %IMAGE_NAME%:latest
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                echo 'Deploying container on Windows machine...'

                bat '''
                    "%DOCKER%" stop deepu 2>NUL || exit 0
                    "%DOCKER%" rm deepu 2>NUL || exit 0

                    "%DOCKER%" pull %IMAGE_NAME%:latest

                    "%DOCKER%" run -d ^
                        --name deepu ^
                        -p 8070:80 ^
                        %IMAGE_NAME%:latest
                '''
            }
        }
    }

    post {
        success {
            echo '======================================'
            echo 'CI/CD Pipeline completed successfully!'
            echo '======================================'
            echo 'Website: http://localhost:8070'
        }

        failure {
            echo '======================================'
            echo 'CI/CD Pipeline failed.'
            echo '======================================'
        }
    }
}

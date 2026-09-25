pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = 'deepu09567'
        IMAGE_NAME = 'deepu09567/staticwebsite_pipleline'

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
                        echo %DOCKER_PASSWORD% | "%DOCKER%" login --username "%DOCKER_USER%" --password-stdin
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
                    "%DOCKER%" stop staticwebsite 2>NUL || exit /B 0

                    "%DOCKER%" rm staticwebsite 2>NUL || exit /B 0

                    "%DOCKER%" pull %IMAGE_NAME%:latest

                    "%DOCKER%" run -d ^
                        --name staticwebsite ^
                        -p 1748:80 ^
                        %IMAGE_NAME%:latest
                '''
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline completed successfully!'
            echo 'Docker Image: deepu09567/staticwebsite_pipleline:latest'
            echo 'Container: staticwebsite'
            echo 'Website: http://localhost:1748'
        }

        failure {
            echo 'CI/CD Pipeline failed.'
        }
    }
}


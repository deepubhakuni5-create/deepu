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

        stage('Docker Environment Test') {
            steps {
                echo 'Checking Jenkins and Docker environment...'

                bat '''
                    whoami
                    echo USERPROFILE=%USERPROFILE%
                    echo DOCKER_CONFIG=%DOCKER_CONFIG%

                    "%DOCKER%" version
                    "%DOCKER%" context show
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

        stage('Docker Credential Test') {
            steps {
                echo 'Testing Jenkins Docker Hub credential...'

                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-deep',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    bat '''
                        if "%DOCKER_USER%"=="" (
                            echo ERROR: Docker username is empty
                            exit /b 1
                        )

                        if "%DOCKER_PASSWORD%"=="" (
                            echo ERROR: Docker password is empty
                            exit /b 1
                        )

                        echo Docker username received from Jenkins:
                        echo %DOCKER_USER%

                        echo Docker password is present.
                    '''
                }
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
                echo 'Deploying container...'

                bat '''
                    echo Stopping old container...

                    "%DOCKER%" stop deepu 2>NUL || exit /b 0

                    echo Removing old container...

                    "%DOCKER%" rm deepu 2>NUL || exit /b 0

                    echo Pulling latest image...

                    "%DOCKER%" pull %IMAGE_NAME%:latest

                    echo Starting new container...

                    "%DOCKER%" run -d ^
                        --name deepu ^
                        -p 8070:80 ^
                        %IMAGE_NAME%:latest
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                echo 'Checking running container...'

                bat '''
                    "%DOCKER%" ps

                    echo.
                    echo ======================================
                    echo Website:
                    echo http://localhost:8070
                    echo ======================================
                '''
            }
        }
    }

    post {
        success {
            echo '''
========================================
CI/CD PIPELINE SUCCESS
========================================
Docker Image:
deepu09567/deepu:latest

Website:
http://localhost:8070
========================================
'''
        }

        failure {
            echo '''
========================================
CI/CD PIPELINE FAILED
========================================
Check the failed stage.
========================================
'''
        }
    }
}

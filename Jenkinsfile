pipeline {
    agent any

    stages {

        stage('Docker Login - Clean Config') {
            steps {

                echo 'Testing Docker Hub login with clean Docker configuration...'

                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {

                    bat '''
                        echo ================================
                        echo Jenkins Docker Environment
                        echo ================================

                        set DOCKER_CONFIG

                        echo.
                        echo ================================
                        echo Creating Clean Docker Config
                        echo ================================

                        if not exist "%WORKSPACE%\\.docker-clean" mkdir "%WORKSPACE%\\.docker-clean"

                        set "DOCKER_CONFIG=%WORKSPACE%\\.docker-clean"

                        echo DOCKER_CONFIG=%DOCKER_CONFIG%

                        echo.
                        echo ================================
                        echo Docker Login
                        echo ================================

                        echo %DOCKER_PASSWORD% | docker login --username "%DOCKER_USER%" --password-stdin

                        echo.
                        echo ================================
                        echo Login Test Complete
                        echo ================================
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Docker Hub Login SUCCESSFUL!'
        }

        failure {
            echo 'Docker Hub Login FAILED!'
        }
    }
}


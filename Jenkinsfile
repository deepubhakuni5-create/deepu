pipeline {
    agent any

    stages {

        stage('Docker Login Test') {
            steps {

                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {

                    bat '''
                        echo =================================
                        echo Docker Hub Login Test
                        echo =================================

                        echo Username: %DOCKER_USER%

                        docker logout

                        echo.
                        echo Starting Docker login...

                        echo %DOCKER_PASSWORD% | docker login docker.io --username "%DOCKER_USER%" --password-stdin

                        if errorlevel 1 (
                            echo.
                            echo Docker Hub LOGIN FAILED
                            exit /b 1
                        )

                        echo.
                        echo Docker Hub LOGIN SUCCESS
                    '''
                }
            }
        }
    }
}


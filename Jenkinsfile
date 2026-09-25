pipeline {
    agent any

    stages {

        stage('Check Docker Login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    bat '''
                        echo ================================
                        echo Jenkins User
                        echo ================================
                        whoami
                        echo.

                        echo USERPROFILE:
                        echo %USERPROFILE%
                        echo.

                        echo Docker Context:
                        docker context show
                        echo.

                        echo Docker Config:
                        if exist "%USERPROFILE%\\.docker\\config.json" (
                            echo Docker config exists
                        ) else (
                            echo Docker config NOT FOUND
                        )
                        echo.

                        echo Docker Login:
                        echo %DOCKER_PASSWORD% | docker login -u "%DOCKER_USER%" --password-stdin
                    '''
                }
            }
        }
    }
}

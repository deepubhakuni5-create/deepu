pipeline {
    agent any

    stages {

        stage('Docker Environment Check') {
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
                        echo JENKINS ENVIRONMENT
                        echo ================================

                        whoami
                        echo.

                        echo USERNAME:
                        echo %USERNAME%
                        echo.

                        echo USERPROFILE:
                        echo %USERPROFILE%
                        echo.

                        echo HOME:
                        echo %HOME%
                        echo.

                        echo HOMEDRIVE:
                        echo %HOMEDRIVE%
                        echo.

                        echo HOMEPATH:
                        echo %HOMEPATH%
                        echo.

                        echo DOCKER_CONFIG:
                        echo %DOCKER_CONFIG%
                        echo.

                        echo DOCKER CONTEXT:
                        docker context show
                        echo.

                        echo DOCKER CONFIG FILE:
                        if exist "%USERPROFILE%\\.docker\\config.json" (
                            echo EXISTS
                        ) else (
                            echo NOT FOUND
                        )
                        echo.

                        echo DOCKER CREDENTIAL:
                        echo Username = %DOCKER_USER%
                        echo Password = PRESENT
                        echo PasswordLength:
                        powershell -NoProfile -Command "$p=$env:DOCKER_PASSWORD; Write-Output $p.Length"
                    '''
                }
            }
        }
    }
}


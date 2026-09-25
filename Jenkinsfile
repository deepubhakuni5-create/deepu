pipeline {
    agent any

    stages {

        stage('Check Jenkins User') {
            steps {
                bat '''
                    echo ================================
                    echo Jenkins Windows User
                    echo ================================

                    whoami

                    echo.
                    echo USERNAME:
                    echo %USERNAME%

                    echo.
                    echo USERPROFILE:
                    echo %USERPROFILE%

                    echo.
                    echo DOCKER CONFIG:
                    echo %DOCKER_CONFIG%

                    echo.
                    echo Docker Context:
                    docker context show

                    echo.
                    echo Docker Version:
                    docker version
                '''
            }
        }
    }
}


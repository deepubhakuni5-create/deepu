pipeline {
    agent any

    stages {

        stage('Docker Context Test') {
            steps {

                bat '''
                    echo =================================
                    echo Docker Context Test
                    echo =================================

                    echo.
                    echo Current Docker Context:
                    docker context show

                    echo.
                    echo Available Docker Contexts:
                    docker context ls

                    echo.
                    echo Switching to desktop-linux...
                    docker context use desktop-linux

                    echo.
                    echo New Docker Context:
                    docker context show

                    echo.
                    echo Docker Info:
                    docker info
                '''
            }
        }
    }
}


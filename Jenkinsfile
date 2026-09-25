pipeline {
    agent any

    stages {

        stage('Check Docker Credential Helper') {
            steps {
                bat '''
                    echo ================================
                    echo Docker Credential Helper
                    echo ================================

                    where docker

                    echo.
                    echo Checking Docker Desktop credential helper...

                    where docker-credential-desktop

                    echo.
                    echo Checking WinCred helper...

                    where docker-credential-wincred

                    echo.
                    echo Docker CLI location:

                    docker --version
                '''
            }
        }
    }
}


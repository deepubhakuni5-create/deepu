pipeline {
    agent any

    stages {

        stage('Test WinCred') {
            steps {
                bat '''
                    echo ================================
                    echo WINCRED TEST
                    echo ================================

                    whoami
                    echo.

                    echo Docker Credential Helper:
                    docker-credential-wincred.exe version
                    echo.

                    echo Stored Docker Credentials:
                    echo {"ServerURL":"https://index.docker.io/v1/"} | docker-credential-wincred.exe get
                '''
            }
        }
    }
}


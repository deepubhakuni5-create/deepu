pipeline {
    agent any

    stages {

        stage('Check Docker Config') {
            steps {
                bat '''
                    echo ================================
                    echo Docker Config Information
                    echo ================================

                    echo User:
                    whoami
                    echo.

                    echo Docker Config File:
                    type "%USERPROFILE%\\.docker\\config.json"
                '''
            }
        }
    }
}

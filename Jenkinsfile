pipeline {
    agent any

    stages {

        stage('Docker Authentication Diagnostic') {
            steps {

                echo 'Checking Docker and Jenkins credentials...'

                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {

                    bat '''
                        echo ================================
                        echo Docker Context
                        echo ================================
                        docker context show

                        echo.
                        echo ================================
                        echo Docker Version
                        echo ================================
                        docker version

                        echo.
                        echo ================================
                        echo Jenkins Credential
                        echo ================================
                        echo Username: %DOCKER_USER%

                        if "%DOCKER_PASSWORD%"=="" (
                            echo PASSWORD IS EMPTY
                        ) else (
                            echo PASSWORD IS PRESENT
                        )

                        echo.
                        echo ================================
                        echo Docker Login
                        echo ================================

                        echo %DOCKER_PASSWORD% | docker login --username "%DOCKER_USER%" --password-stdin
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Docker login successful!'
        }

        failure {
            echo 'Docker login failed. Check the console output.'
        }
    }
}

pipeline {
    agent any

    stages {

        stage('Test Docker Credential Helper') {
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
                        echo Docker Credential Helper Test
                        echo ================================

                        echo Jenkins User:
                        whoami
                        echo.

                        echo Docker Credential Helper:
                        docker-credential-desktop.exe version
                        echo.

                        echo Testing Docker Login:
                        echo %DOCKER_PASSWORD% | docker login -u "%DOCKER_USER%" --password-stdin
                    '''
                }
            }
        }
    }
}

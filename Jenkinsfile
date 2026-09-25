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
                        echo %DOCKER_PASSWORD% | docker login -u "%DOCKER_USER%" --password-stdin
                    '''
                }
            }
        }
    }
}

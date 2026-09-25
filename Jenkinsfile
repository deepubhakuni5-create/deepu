pipeline {
    agent any

    stages {

        stage('Docker Login') {
            steps {

                echo 'Testing Docker Hub login from Jenkins...'

                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {

                    bat '''
                        echo Username: %DOCKER_USER%
                        echo Password: dckr_pat_XGsQWtLlUurU9-B6J9kn-0q7vj8

                        echo %DOCKER_PASSWORD% | docker login --username "%DOCKER_USER%" --password-stdin
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Docker Hub Login SUCCESSFUL!'
        }

        failure {
            echo 'Docker Hub Login FAILED!'
        }
    }
}


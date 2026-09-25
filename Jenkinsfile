pipeline {
    agent any

    stages {

        stage('Docker Hub Login Test') {
            steps {

                echo 'Testing Docker Hub authentication...'

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
                    echo Docker Hub Login
                    echo ================================
                    echo.

                    docker login -u deepu09567
                '''
            }
        }
    }

    post {
        success {
            echo 'Docker Hub login successful!'
        }

        failure {
            echo 'Docker Hub login failed.'
        }
    }
}


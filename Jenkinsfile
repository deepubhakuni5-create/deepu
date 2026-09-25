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

                    powershell '''
                        Write-Host "================================"
                        Write-Host "Docker Login"
                        Write-Host "================================"

                        Write-Host "Username: $env:DOCKER_USER"
                        Write-Host "Password: PRESENT"

                        $env:DOCKER_PASSWORD | docker login `
                            --username $env:DOCKER_USER `
                            --password-stdin

                        if ($LASTEXITCODE -ne 0) {
                            Write-Host "Docker login failed."
                            exit $LASTEXITCODE
                        }

                        Write-Host "Docker login successful!"
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


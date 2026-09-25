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
                        Write-Host "Docker Hub Login"
                        Write-Host "================================"

                        Write-Host "Username: $env:DOCKER_USER"
                        Write-Host "Password Present: $([string]::IsNullOrEmpty($env:DOCKER_PASSWORD) -eq $false)"
                        Write-Host "Password Length: $($env:DOCKER_PASSWORD.Length)"

                        Write-Host ""
                        Write-Host "Starting Docker Login..."

                        $env:DOCKER_PASSWORD | docker login --username $env:DOCKER_USER --password-stdin

                        if ($LASTEXITCODE -ne 0) {
                            Write-Host ""
                            Write-Host "Docker Hub Login FAILED"
                            exit $LASTEXITCODE
                        }

                        Write-Host ""
                        Write-Host "Docker Hub Login SUCCESSFUL"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Docker Hub authentication is working!'
        }

        failure {
            echo 'Docker Hub authentication failed!'
        }
    }
}
```

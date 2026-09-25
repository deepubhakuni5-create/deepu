pipeline {
    agent any

    stages {

        stage('Docker Login') {
            steps {

                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {

                    powershell '''
                        Write-Host "================================"
                        Write-Host "Docker Hub Login Test"
                        Write-Host "================================"

                        Write-Host "Username: $env:DOCKER_USER"
                        Write-Host "Password Present: $([string]::IsNullOrEmpty($env:DOCKER_PASSWORD) -eq $false)"
                        Write-Host "Password Length: $($env:DOCKER_PASSWORD.Length)"

                        $env:DOCKER_PASSWORD | docker login `
                            --username $env:DOCKER_USER `
                            --password-stdin

                        if ($LASTEXITCODE -ne 0) {
                            Write-Host "Docker Hub login FAILED"
                            exit $LASTEXITCODE
                        }

                        Write-Host "Docker Hub login SUCCESSFUL"
                    '''
                }
            }
        }
    }
}


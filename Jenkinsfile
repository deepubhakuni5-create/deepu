pipeline {
    agent any

    stages {

        stage('Docker Login - Clean Config') {
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
                        Write-Host "Docker Login - Clean Config"
                        Write-Host "================================"

                        $cleanConfig = "$env:WORKSPACE/.docker-test"

                        if (Test-Path $cleanConfig) {
                            Remove-Item $cleanConfig -Recurse -Force
                        }

                        New-Item -ItemType Directory -Path $cleanConfig -Force | Out-Null

                        $env:DOCKER_CONFIG = $cleanConfig

                        Write-Host "Docker Config:"
                        Write-Host $env:DOCKER_CONFIG

                        Write-Host ""
                        Write-Host "Username:"
                        Write-Host $env:DOCKER_USER

                        Write-Host ""
                        Write-Host "Password Present:"
                        Write-Host ([string]::IsNullOrEmpty($env:DOCKER_PASSWORD) -eq $false)

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
            echo 'Docker Hub login successful with clean Docker config!'
        }

        failure {
            echo 'Docker Hub login failed even with clean Docker config.'
        }
    }
}


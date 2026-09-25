pipeline {
    agent any

    stages {

        stage('Clean Docker Login Test') {
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
                        Write-Host "Clean Docker Login Test"
                        Write-Host "================================"

                        $cleanConfig = "$env:WORKSPACE/docker-config-test"

                        if (Test-Path $cleanConfig) {
                            Remove-Item $cleanConfig -Recurse -Force
                        }

                        New-Item -ItemType Directory -Path $cleanConfig -Force | Out-Null

                        $env:DOCKER_CONFIG = $cleanConfig

                        Write-Host "Docker Config:"
                        Write-Host $env:DOCKER_CONFIG

                        Write-Host ""
                        Write-Host "Docker Version:"
                        docker version --format "{{.Client.Version}} / {{.Server.Version}}"

                        Write-Host ""
                        Write-Host "Starting Docker Login..."

                        $env:DOCKER_PASSWORD | docker login docker.io --username $env:DOCKER_USER --password-stdin

                        if ($LASTEXITCODE -ne 0) {
                            Write-Host ""
                            Write-Host "Docker LOGIN FAILED"
                            exit 1
                        }

                        Write-Host ""
                        Write-Host "Docker LOGIN SUCCESS"

                        Write-Host ""
                        Write-Host "Docker Config Created:"
                        Get-ChildItem $cleanConfig
                    '''
                }
            }
        }
    }
}


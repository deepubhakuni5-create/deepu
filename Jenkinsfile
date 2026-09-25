pipeline {
    agent any

    stages {

        stage('Create Docker Auth') {
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
                        Write-Host "Create Docker Authentication"
                        Write-Host "================================"

                        $dockerConfig = "$env:WORKSPACE/docker-auth-test"

                        if (Test-Path $dockerConfig) {
                            Remove-Item $dockerConfig -Recurse -Force
                        }

                        New-Item -ItemType Directory -Path $dockerConfig -Force | Out-Null

                        $pair = "$($env:DOCKER_USER):$($env:DOCKER_PASSWORD)"

                        $bytes = [System.Text.Encoding]::UTF8.GetBytes($pair)

                        $auth = [Convert]::ToBase64String($bytes)

                        $config = @{
                            auths = @{
                                "https://index.docker.io/v1/" = @{
                                    auth = $auth
                                }
                            }
                        }

                        $configFile = "$dockerConfig/config.json"

                        $config | ConvertTo-Json -Depth 5 |
                            Set-Content -Path $configFile -Encoding UTF8

                        $env:DOCKER_CONFIG = $dockerConfig

                        Write-Host ""
                        Write-Host "Docker Config:"
                        Write-Host $configFile

                        Write-Host ""
                        Write-Host "Docker Context:"
                        docker context show

                        Write-Host ""
                        Write-Host "Testing Docker Hub access..."

                        docker pull hello-world

                        if ($LASTEXITCODE -ne 0) {
                            Write-Host ""
                            Write-Host "Docker authentication/access test FAILED"
                            exit 1
                        }

                        Write-Host ""
                        Write-Host "Docker authentication configuration loaded successfully."
                    '''
                }
            }
        }
    }
}


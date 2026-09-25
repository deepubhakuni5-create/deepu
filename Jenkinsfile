pipeline {
    agent any

    stages {

        stage('Docker Private Registry Test') {
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
                        Write-Host "Docker Private Registry Test"
                        Write-Host "================================"

                        $dockerConfig = "$env:WORKSPACE/docker-auth-test"

                        if (Test-Path $dockerConfig) {
                            Remove-Item $dockerConfig -Recurse -Force
                        }

                        New-Item -ItemType Directory -Path $dockerConfig -Force | Out-Null

                        $pair = "$($env:DOCKER_USER):$($env:DOCKER_PASSWORD)"
                        $bytes = [System.Text.Encoding]::UTF8.GetBytes($pair)
                        $auth = [Convert]::ToBase64String($bytes)

                        $configObject = @{
                            auths = @{
                                "https://index.docker.io/v1/" = @{
                                    auth = $auth
                                }
                            }
                        }

                        $json = $configObject | ConvertTo-Json -Depth 5

                        # UTF-8 WITHOUT BOM
                        $utf8NoBom = New-Object System.Text.UTF8Encoding($false)

                        [System.IO.File]::WriteAllText(
                            "$dockerConfig/config.json",
                            $json,
                            $utf8NoBom
                        )

                        $env:DOCKER_CONFIG = $dockerConfig

                        Write-Host ""
                        Write-Host "Docker Config:"
                        Get-Content "$dockerConfig/config.json" |
                            ForEach-Object {
                                $_ -replace $auth, "AUTH_MASKED"
                            }

                        Write-Host ""
                        Write-Host "Testing private Docker Hub authentication..."

                        docker pull $env:DOCKER_USER/staticside:latest

                        if ($LASTEXITCODE -ne 0) {
                            Write-Host ""
                            Write-Host "PRIVATE REGISTRY TEST FAILED"
                            exit 1
                        }

                        Write-Host ""
                        Write-Host "PRIVATE REGISTRY TEST SUCCESS"
                    '''
                }
            }
        }
    }
}


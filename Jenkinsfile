pipeline {
    agent any

    stages {

        stage('LocalSystem Docker Diagnostic') {
            steps {

                powershell '''
                    Write-Host "================================"
                    Write-Host "Jenkins Docker Diagnostic"
                    Write-Host "================================"

                    Write-Host ""
                    Write-Host "Windows User:"
                    whoami

                    Write-Host ""
                    Write-Host "Docker Version:"
                    docker version --format "{{.Client.Version}} / {{.Server.Version}}"

                    Write-Host ""
                    Write-Host "Docker Context:"
                    docker context show

                    Write-Host ""
                    Write-Host "Docker Config:"
                    if ($env:DOCKER_CONFIG) {
                        Write-Host "DOCKER_CONFIG = $env:DOCKER_CONFIG"
                    }
                    else {
                        Write-Host "DOCKER_CONFIG = NOT SET"
                    }

                    Write-Host ""
                    Write-Host "USERPROFILE:"
                    Write-Host $env:USERPROFILE

                    Write-Host ""
                    Write-Host "APPDATA:"
                    Write-Host $env:APPDATA

                    Write-Host ""
                    Write-Host "Docker config locations:"

                    $paths = @(
                        "$env:USERPROFILE\.docker\config.json",
                        "$env:APPDATA\Docker\config.json",
                        "C:\Windows\System32\config\systemprofile\.docker\config.json"
                    )

                    foreach ($path in $paths) {
                        if (Test-Path $path) {
                            Write-Host "FOUND: $path"
                        }
                        else {
                            Write-Host "NOT FOUND: $path"
                        }
                    }
                '''
            }
        }
    }
}


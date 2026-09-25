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
                    Write-Host "DOCKER_CONFIG:"
                    if ($env:DOCKER_CONFIG) {
                        Write-Host $env:DOCKER_CONFIG
                    }
                    else {
                        Write-Host "NOT SET"
                    }

                    Write-Host ""
                    Write-Host "USERPROFILE:"
                    Write-Host $env:USERPROFILE

                    Write-Host ""
                    Write-Host "APPDATA:"
                    Write-Host $env:APPDATA

                    Write-Host ""
                    Write-Host "Docker configuration:"
                    
                    if (Test-Path "$env:USERPROFILE/.docker/config.json") {
                        Write-Host "FOUND USERPROFILE Docker config"
                    }
                    else {
                        Write-Host "USERPROFILE Docker config NOT FOUND"
                    }

                    if (Test-Path "C:/Windows/System32/config/systemprofile/.docker/config.json") {
                        Write-Host "FOUND LocalSystem Docker config"
                    }
                    else {
                        Write-Host "LocalSystem Docker config NOT FOUND"
                    }
                '''
            }
        }
    }
}

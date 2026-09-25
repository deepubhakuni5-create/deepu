pipeline {
    agent any

    stages {

        stage('Docker Hub Connectivity') {
            steps {

                powershell '''
                    Write-Host "================================"
                    Write-Host "Docker Hub Connectivity Test"
                    Write-Host "================================"

                    Write-Host "Docker:"
                    docker --version

                    Write-Host ""
                    Write-Host "Docker Context:"
                    docker context show

                    Write-Host ""
                    Write-Host "Docker Hub API:"
                    try {
                        $response = Invoke-WebRequest `
                            -Uri "https://registry-1.docker.io/v2/" `
                            -UseBasicParsing

                        Write-Host "HTTP Status: $($response.StatusCode)"
                    }
                    catch {
                        Write-Host "HTTP Status: $($_.Exception.Response.StatusCode.value__)"
                        Write-Host "Docker Hub is reachable."
                    }
                '''
            }
        }
    }
}


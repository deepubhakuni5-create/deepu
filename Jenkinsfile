pipeline {
    agent any

    stages {

        stage('Docker Hub Credential Test') {
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
                        Write-Host "Docker Hub Credential Test"
                        Write-Host "================================"

                        Write-Host "Username: $env:DOCKER_USER"
                        Write-Host "Password Present: $([string]::IsNullOrEmpty($env:DOCKER_PASSWORD) -eq $false)"
                        Write-Host "Password Length: $($env:DOCKER_PASSWORD.Length)"

                        $pair = "$($env:DOCKER_USER):$($env:DOCKER_PASSWORD)"
                        $encoded = [Convert]::ToBase64String(
                            [Text.Encoding]::UTF8.GetBytes($pair)
                        )

                        $headers = @{
                            Authorization = "Basic $encoded"
                        }

                        try {
                            $response = Invoke-WebRequest `
                                -Uri "https://hub.docker.com/v2/users/login/" `
                                -Method POST `
                                -Headers $headers `
                                -UseBasicParsing

                            Write-Host ""
                            Write-Host "Docker Hub Authentication HTTP Status:"
                            Write-Host $response.StatusCode

                            Write-Host ""
                            Write-Host "Docker Hub authentication request completed."
                        }
                        catch {
                            Write-Host ""
                            Write-Host "HTTP Status:"
                            Write-Host $_.Exception.Response.StatusCode.value__

                            Write-Host ""
                            Write-Host "Docker Hub rejected the credentials."
                        }
                    '''
                }
            }
        }
    }
}


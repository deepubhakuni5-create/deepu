pipeline {
    agent any

    stages {

        stage('Validate Docker Hub PAT') {
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
                        Write-Host "Docker Hub PAT Validation"
                        Write-Host "================================"

                        Write-Host "Username: $env:DOCKER_USER"
                        Write-Host "Password Present: $([string]::IsNullOrEmpty($env:DOCKER_PASSWORD) -eq $false)"
                        Write-Host "Password Length: $($env:DOCKER_PASSWORD.Length)"

                        $pair = "$($env:DOCKER_USER):$($env:DOCKER_PASSWORD)"
                        $bytes = [System.Text.Encoding]::UTF8.GetBytes($pair)
                        $encoded = [Convert]::ToBase64String($bytes)

                        $headers = @{
                            Authorization = "Basic $encoded"
                        }

                        try {

                            $response = Invoke-WebRequest `
                                -Uri "https://auth.docker.io/token?service=registry.docker.io&scope=repository:library/nginx:pull" `
                                -Method GET `
                                -Headers $headers `
                                -UseBasicParsing

                            Write-Host ""
                            Write-Host "HTTP Status:"
                            Write-Host $response.StatusCode

                            Write-Host ""
                            Write-Host "Docker Hub PAT ACCEPTED"

                        }
                        catch {

                            Write-Host ""
                            Write-Host "HTTP Status:"
                            Write-Host $_.Exception.Response.StatusCode.value__

                            Write-Host ""
                            Write-Host "Docker Hub PAT REJECTED"
                            exit 1
                        }
                    '''
                }
            }
        }
    }
}


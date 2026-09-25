pipeline {
    agent any

    stages {

        stage('Check Docker Credential') {
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
                        Write-Host "Jenkins Credential Test"
                        Write-Host "================================"

                        Write-Host "Username: $env:DOCKER_USER"
                        Write-Host "Password Present: $([string]::IsNullOrEmpty($env:DOCKER_PASSWORD) -eq $false)"
                        Write-Host "Password Length: $($env:DOCKER_PASSWORD.Length)"

                        docker --version
                        docker context show
                    '''
                }
            }
        }
    }
}


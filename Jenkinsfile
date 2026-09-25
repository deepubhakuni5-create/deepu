pipeline {
    agent any

    stages {

        stage('Exact Docker Login Test') {
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
                        Write-Host "Exact Docker Login Test"
                        Write-Host "================================"

                        $testDir = "$env:WORKSPACE/docker-login-test"

                        if (Test-Path $testDir) {
                            Remove-Item $testDir -Recurse -Force
                        }

                        New-Item -ItemType Directory -Path $testDir -Force | Out-Null

                        $passwordFile = "$testDir/password.txt"

                        # Write the Jenkins credential exactly as UTF-8
                        $utf8NoBom = New-Object System.Text.UTF8Encoding($false)
                        [System.IO.File]::WriteAllText(
                            $passwordFile,
                            $env:DOCKER_PASSWORD,
                            $utf8NoBom
                        )

                        Write-Host "Username: $env:DOCKER_USER"
                        Write-Host "Password Present: $([string]::IsNullOrEmpty($env:DOCKER_PASSWORD) -eq $false)"
                        Write-Host "Password Length: $($env:DOCKER_PASSWORD.Length)"

                        Write-Host ""
                        Write-Host "Starting Docker Login..."

                        cmd.exe /c "type `"$passwordFile`" | docker login docker.io --username `"$env:DOCKER_USER`" --password-stdin"

                        if ($LASTEXITCODE -ne 0) {
                            Write-Host ""
                            Write-Host "Docker LOGIN FAILED"
                            exit 1
                        }

                        Write-Host ""
                        Write-Host "Docker LOGIN SUCCESS"

                        Remove-Item $passwordFile -Force
                    '''
                }
            }
        }
    }
}


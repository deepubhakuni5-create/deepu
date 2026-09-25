pipeline {
    agent any

    environment {
        IMAGE_NAME = 'deepu09567/staticside'
        IMAGE_TAG  = 'latest'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'

                git branch: 'main',
                    url: 'https://github.com/deepubhakuni5-create/deepu.git'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'

                bat '''
                    docker build -t %IMAGE_NAME%:%IMAGE_TAG% .
                '''
            }
        }

        stage('Docker Login') {
            steps {
                echo 'Logging into Docker Hub...'

                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {

                    powershell '''
                        $dockerConfig = "$env:WORKSPACE/docker-auth"

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

                        # Write UTF-8 WITHOUT BOM
                        $utf8NoBom = New-Object System.Text.UTF8Encoding($false)

                        [System.IO.File]::WriteAllText(
                            "$dockerConfig/config.json",
                            $json,
                            $utf8NoBom
                        )

                        $env:DOCKER_CONFIG = $dockerConfig

                        Write-Host "Docker Hub authentication configured successfully."

                        docker info

                        if ($LASTEXITCODE -ne 0) {
                            Write-Host "Docker is not available."
                            exit 1
                        }
                    '''
                }
            }
        }

        stage('Docker Push') {
            steps {
                echo 'Pushing Docker image to Docker Hub...'

                powershell '''
                    $dockerConfig = "$env:WORKSPACE/docker-auth"
                    $env:DOCKER_CONFIG = $dockerConfig

                    docker push "$env:IMAGE_NAME`:$env:IMAGE_TAG"

                    if ($LASTEXITCODE -ne 0) {
                        Write-Host "Docker push failed."
                        exit 1
                    }

                    Write-Host "Docker image pushed successfully."
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                echo 'Deploying website container...'

                bat '''
                    docker stop staticwebsite >NUL 2>&1
                    docker rm staticwebsite >NUL 2>&1

                    docker pull %IMAGE_NAME%:%IMAGE_TAG%

                    docker run -d ^
                        --name staticwebsite ^
                        -p 1748:80 ^
                        %IMAGE_NAME%:%IMAGE_TAG%
                '''
            }
        }
    }

    post {
        success {
            echo '======================================'
            echo 'CI/CD PIPELINE SUCCESSFUL'
            echo '======================================'
            echo 'Docker Image: deepu09567/staticside:latest'
            echo 'Container: staticwebsite'
            echo 'Website: http://localhost:1748'
        }

        failure {
            echo '======================================'
            echo 'CI/CD PIPELINE FAILED'
            echo '======================================'
        }

        always {
            powershell '''
                $dockerConfig = "$env:WORKSPACE/docker-auth"

                if (Test-Path $dockerConfig) {
                    Remove-Item $dockerConfig -Recurse -Force
                }
            '''
        }
    }
}


pipeline {
    agent any

    stages {
        stage('Setup .NET Core') {
            agent {
                docker {
                    image 'mcr.microsoft.com/dotnet/sdk:6.0'
                    args '-u root:root'
                    reuseNode true
                }
            }
            steps {
                bat '''
                    echo Installing .NET Core SDK 6.0
                    choco install dotnet-sdk --version=6.0.100 -y
                '''
            }
        }
        stage('Restore Dependencies') {
            docker {
                image 'mcr.microsoft.com/dotnet/sdk:6.0'
                args '-u root:root'
                reuseNode true
            }
            steps {
                bat '''
                    echo Restoring NuGet packages
                    dotnet restore
                '''
            }
        }
        stage('Build') {
            docker {
                image 'mcr.microsoft.com/dotnet/sdk:6.0'
                args '-u root:root'
                reuseNode true
            }
            steps {
                bat '''
                    echo Building the project
                    dotnet build
                '''
            }
        }
        stage('Test') {
            docker {
                image 'mcr.microsoft.com/dotnet/sdk:6.0'
                args '-u root:root'
                reuseNode true
            }
            steps {
                bat '''
                    echo Running tests
                    dotnet test
                '''
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true
            junit '**/TestResults/*.trx'
        }
    }
}
pipeline {
    agent any

    stages {
        stage('Setup .NET Core') {
            agent {
                docker {
                    image 'mcr.microsoft.com/dotnet/sdk:6.0'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo Installing .NET Core SDK 6.0
                    choco install dotnet-sdk --version=6.0.100 -y
                '''
            }
        }
        stage('Restore Dependencies') {
            agent {
                docker {
                    image 'mcr.microsoft.com/dotnet/sdk:6.0'
                    args '-u root:root'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo Restoring NuGet packages
                    dotnet restore
                '''
            }
        }
        stage('Build') {
            agent {
                docker {
                    image 'mcr.microsoft.com/dotnet/sdk:6.0'
                    args '-u root:root'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo Building the project
                    dotnet build
                '''
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'mcr.microsoft.com/dotnet/sdk:6.0'
                    args '-u root:root'
                    reuseNode true
                }
            }
            steps {
                sh '''
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
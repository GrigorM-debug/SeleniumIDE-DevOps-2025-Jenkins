pipeline {
    agent any

    options {
        skipDefaultCheckout()
    }

    stages {
        stage('Restore Dependencies') {
            steps {
                bat '''
                    echo Restoring NuGet packages
                    dotnet restore
                '''
            }
        }
        stage('Build') {
            steps {
                bat '''
                    echo Building the project
                    dotnet build
                '''
            }
        }
        stage('Test') {
            steps {
                bat '''
                    echo Running tests
                    dotnet test --logger trx;LogFileName=TestResults.trx
                '''
            }
        }
    }

    post {
        always {
            archiveArtifacts '**/TestResults/*.trx'
            publishChecks name: 'Jenkins Build'  // Sends details to GitHub Checks
        }
    }
}
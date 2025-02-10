pipeline {
    agent any

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
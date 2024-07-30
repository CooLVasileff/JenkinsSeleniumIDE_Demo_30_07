pipeline {
    agent any

    stages {
        stage("Checkout code") {
            steps {
                // Checkout the repository
                git branch: 'main', url: "https://github.com/CooLVasileff/JenkinsSeleniumIDE_Demo_30_07"
            }
        }
        stage("Set up .Net core") {
            steps {
                bat '''
                echo Downloading .Net 6 SDK
                curl -L -o dotnet-sdk-6.0.132-win-x64.exe https://download.visualstudio.microsoft.com/download/pr/0c82e7e6-fdde-49f2-a69f-bd986aeafe1b/9ea7411a22e661fff0e61e56a466e484/dotnet-sdk-6.0.132-win-x64.exe
                echo Installing SDK
                dotnet-sdk-6.0.132-win-x64.exe /quiet /norestart
                '''
            }
        }
        stage("Restoring nuget packages") {
            steps {
                // Restore dependencies
                bat 'dotnet restore SeleniumIde.sln'
            }
        }
        stage("Build") {
            steps {
                // Build solution
                bat 'dotnet build SeleniumIde.sln --configuration Release'
            }
        }
        stage("Run tests") {
            steps {
                // Run tests
                bat 'dotnet test SeleniumIde.sln --logger "trx;LogFileName=TestResults.trx"'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true
            mstest testResultsFile: '**/TestResults/*.trx'
        }
    }
}

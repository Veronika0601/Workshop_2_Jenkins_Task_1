pipeline {
    agent any

    stages {
        stage("Checkout git code") {
            steps {
                git branch: "main", url: "https://github.com/Veronika0601/Workshop_2_Jenkins_Task_1"
            }
        }

        stage("Setup dotnet 6") {
            steps {
                bat '''
                curl --ssl-no-revoke -L -o dotnet-sdk-6.0.136-win-x86.exe https://download.visualstudio.microsoft.com/download/pr/4d3e3aaf-755f-4a17-b8fc-6f8e924a5b26/e2df2be9b11dd8884df40ee27239d02d/dotnet-sdk-6.0.136-win-x86.exe
                dotnet-sdk-6.0.136-win-x86.exe /quiet /norestart
                '''
            }
        }

        stage("Install nuget packages") {
            steps {
                bat "dotnet restore SeleniumIde.sln"
            }
        }

        stage("Build the project") {
            steps {
                bat "dotnet build SeleniumIde.sln"
            }
        }

        stage("Run tests") {
            steps {
                bat 'dotnet test SeleniumIde.sln --logger "trx;LogFileName=TestResults.trx"'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true
            // Ако искаш да използваш MStestPublisher, увери се, че plugin-ът е инсталиран
            // В противен случай закоментирай реда по-долу:
            step([
                $class: 'MSTestPublisher',
                testResultsFile: '**/TestResults/*.trx'
            ])
        }
    }
}

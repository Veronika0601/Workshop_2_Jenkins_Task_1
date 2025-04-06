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
                choco install dotnet-sdk -y --version=6.0.100
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
            archiveArtifacts artifacts: '**/TestResults/*.trx',
	    allowEmptyArchive: true
            step([
                $class: 'MStestPublisher',
                testResultsFile: '**/TestResults/*.trx'
            ])
        }
    }
}

pipeline {
  agent any

  stages {
      stage('Checkout code') {
        // checkout the repository
        steps {
          git branch: 'main', url: 'https://github.com/Iliya1914/Selenium-WebDriver-Demo-Jenkins-CI-Workflow'
        }
      }

      stage('Set up .NET Core') {
        steps {
            bat '''
            echo Downloading .NET SDK 6.0
            curl -l -o dotnet-sdk-6.0.132-win-x64.exe https://download.visualstudio.microsoft.com/download/pr/0c82e7e6-fdde-49f2-a69f-bd986aeafe1b/9ea7411a22e661fff0e61e56a466e484/dotnet-sdk-6.0.132-win-x64.exe
            echo Installing dotnet-sdk-6.0.132-win-x64.exe
            dotnet-sdk-6.0.132-win-x64.exe /quiet /norestart
            '''
        }
      }

      stage('Restoring NuGet Packages') {
        steps {
          bat 'dotnet restore SeleniumBasicExercise.sln'
        }
      }

      stage('Build') {
        steps {
          bat 'dotnet build SeleniumBasicExercise.sln --configuration Release'
        }
      }

      stage('Run Tests TestProject1') {
        steps {
          bat 'dotnet test TestProject1/TestProject1.csproj --logger "trx;LogFileName=TestResults.trx"'
        }
      }

      stage('Run Tests TestProject2') {
        steps {
          bat 'dotnet test TestProject2/TestProject2.csproj --logger "trx;LogFileName=TestResults.trx"'
        }
      }

      stage('Run Tests TestProject3') {
        steps {
          bat 'dotnet test TestProject3/TestProject3.csproj --logger "trx;LogFileName=TestResults.trx"'
        }
      }
  }

  post {
    always {
      archiveArtifacts artifacts: '**/TestResults*.trx', allowEmptyArchive: true
      step ([
          $class: 'MSTestPublisher',
          testResultsFile: '**/TestResults*.trx'
      ])
    }
  }
}
pipeline{
  agent any
  stages{
    stage('Checkout code'){
      steps{
        git branch: 'main', url: 'https://github.com/CrimsonKingCodes/Jenkins2025AprilTest'
      }
    }

    stage('Setup DotNet core'){
      steps{
        bat '''
        echo Downloading .Net 6 Sdk
        curl -l -o dotnet-sdk-6.0.428-win-x86.exe https://builds.dotnet.microsoft.com/dotnet/Sdk/6.0.428/dotnet-sdk-6.0.428-win-x86.exe
        echo installing dotnet-sdk-6.0.428-win-x86.exe
        dotnet-sdk-6.0.428-win-x86.exe /quiet /norestart
        '''
      }
    }

    stage('SRestoring NuGet packages'){
      steps{
        bat 'dotnet restore SeleniumBasicExercise.sln'
      }
    }

    stage('Build'){
      steps{
        bat 'dotnet build SeleniumBasicExercise.sln --configuration Release'
      }
    }

    stage('Run Tests TestProject1'){
      steps{
        bat 'dotnet test TestProject1/TestProject1.csproj --logger "trx;LogFileName=TestResults.trx"'
      }
    }

    stage('Run Tests TestProject2'){
      steps{
        bat 'dotnet test TestProject1/TestProject2.csproj --logger "trx;LogFileName=TestResults.trx"'
      }
    }

    stage('Run Tests TestProject3'){
      steps{
        bat 'dotnet test TestProject1/TestProject3.csproj --logger "trx;LogFileName=TestResults.trx"'
      }
    }
  }

  post{
    always{
      archiveArtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true
      step([
        $class: 'MSTestPublisher',
        TestResultsFile: '**/TestResults/*.trx'
      ])
    }
  }
}
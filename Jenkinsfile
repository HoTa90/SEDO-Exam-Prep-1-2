pipeline {
    agent any

    triggers {
 
        pollSCM('H/5 * * * *')
    }


    stages {
        stage('Checkout Repo') {
            when {
                anyOf {
                    branch 'main'
                    branch pattern: "feature.*", comparator: "REGEXP"
                }
            }
            steps {
                checkout scm
            }
        }

        stage('Setup .NET') {
            steps {
                sh 'dotnet --version || sudo apt-get update && sudo apt-get install -y dotnet-sdk-6.0'
            }
        }

        stage('Restore dependencies') {
            steps {
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build --no-restore'
            }
        }

        stage('Test') {
            steps {
                sh 'dotnet test --no-build --verbosity normal'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
    }
}

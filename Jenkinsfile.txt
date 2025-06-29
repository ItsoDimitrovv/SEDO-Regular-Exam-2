pipeline {
    agent any

    triggers {
        pollSCM('H/2 * * * *') // Poll for changes every 2 minutes
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Restore') {
            steps {
                bat 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                bat 'dotnet build --no-restore'
            }
        }

        stage('Execute Test') {
            steps {
                bat 'dotnet test --no-build --verbosity normal'
            }
        }
    }
}

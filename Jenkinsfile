pipeline {

    agent any

    options {
        buildDiscarder logRotator(
                daysToKeepStr: '16',
                numToKeepStr: '10'
        )
    }

    stages {

        stage('Cleanup Workspace') {
            steps {
                cleanWs()
                bat """
                echo "Cleaned Up Workspace For Project"
                """
                bat """
                mvn clean package -DskipTests=true
                """
            }
        }

        stage('Code Checkout') {
            steps {
                checkout([
                        $class           : 'GitSCM',
                        branches         : [[name: '*/main']],
                        userRemoteConfigs: [[url: 'https://github.com/ggazila/jenkins-build.git']]
                ])
            }
        }

        stage(' Unit Testing') {
            steps {
                bat """
                echo "Running Unit Tests"
                """
                bat """
                mvn test
                """
            }
        }

        stage('Code Analysis') {
            steps {
                bat """
                echo "Running Code Analysis"
                """
            }
        }

        stage('Build Deploy Code') {
            when {
                branch 'develop'
            }
            steps {
                bat """
                echo "Building Artifact"
                """

                bat """
                echo "Deploying Code"
                """
            }
        }

    }
}

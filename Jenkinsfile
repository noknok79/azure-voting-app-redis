pipeline {
    agent any

    stages {
        stage('Verify Branch') {
            steps {
                echo "$GIT_BRANCH" 
            }
        }
        stage('Docker  ') {
            steps {
                sh '''docker images -a
                cd azure-vote/
                docker image -a
                docker build -t jenkins-pipeline
                docker image -a
                cd ..'''
            }
        }
    }
}
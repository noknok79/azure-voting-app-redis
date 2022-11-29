pipeline {
    agent any

    stages {
        stage('Verify Branch') {
            steps {
                echo "$GIT_BRANCH" 
            }
        }
        stage('Docker ') {
            steps {
                pwsh 'docker images -a'
            }
        }
    }
}
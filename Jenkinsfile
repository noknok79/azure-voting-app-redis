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
                docker images -a
                docker build .
                docker images -a
                cd ..'''
            }
        }
        stage('START Test App  ') {
            steps {
                sh '''
                docker-compose up -d
                ./scripts/test_container.sh
                '''
            }
            post {
                success {
                    echo 'App started successfully'
                }
                failure {
                    echo 'App failed to start'
                    sh '''docker-compose down'''
                }
            }
        }
        stage('Run Test  ') {
            steps {
                sh '''python ./tests/test_sample.py'''
            }
        }
        stage('Stop Test app  ') {
            steps {
                sh '''docker-compose down'''
            }
        }
        stage('Push to Docker Hub  ') {
            steps {
                sh '''
                docker tag azure-vote-front noknok79/azure-vote-front:latest
                docker push noknok79/azure-vote-front:latest
                '''
            }
        }
    }
}
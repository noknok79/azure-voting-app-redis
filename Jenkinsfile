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
                sh '''docker login -u $DOCKER_USER -p $DOCKER_PASS
                docker tag azure-vote-front $DOCKER_USER/azure-vote-front
                docker push $DOCKER_USER/azure-vote-front
                docker logout'''
            }
        }
    }
}
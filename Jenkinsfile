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
        stage('Push Container  ') {
            steps {
                echo "Workspace is $WORKSPACE"
		        dir("$WORKSPACE/azure-vote"){
                   script {
                      docker.withRegistry('https://index.docker.io/v1/', 'DockerHub') {
                         def image = docker.build('noknok79/jenkins-test:latest')
                         image.push()  
                      }                  
                   }
                }
            }
        }
    }
}
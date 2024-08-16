pipeline {
    agent any

    stages {
        stage('w/o docker') {
            steps {
                sh '''
                echo "without docker"
                ls -la
                touch no-cont.txt
                '''
            }
        }
        
        stage('with docker') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                echo "with docker"
                npm --version
                ls -la
                touch cont.txt
                '''
            }
        }
    }
}

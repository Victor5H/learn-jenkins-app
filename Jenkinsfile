pipeline {
    agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

    stages {
        stage('build') {
            
            steps {
                sh '''
                ls -la
                node --version
                npm --version
                npm ci
                npm run build
                '''
            }
        }
        stage("Test"){
            steps{
                sh '''
                if [ -e build/index.html ]
                    then echo "present"
                else
                    echo "not present"
                fi
                npm test
                '''
            }
        }
        
    }
}

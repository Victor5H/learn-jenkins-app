pipeline {
    agent any
    stages {
        /*
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
        */
        stage("Test"){
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
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
        stage("E2E"){
            agent{
                docker{
                    image 'mcr.microsoft.com/playwright:v1.46.0-jammy'
                    reuseNode true
                }
            }
            steps{
                sh '''
                npm install serve
                node_modules/.bin/serve/serve -s build
                npx playwright test
                '''
            }
        }
        
    }
    post{
        always{
            junit 'test-results/junit.xml'
        }
    }
}

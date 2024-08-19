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
                node_modules/.bin/serve -s build &
                sleep 10 
                npx playwright install chromium
                npx playwright test
                '''
            }
        }
        
    }
    post{
        always{
            junit 'jest-results/junit.xml'
        }
    }
}

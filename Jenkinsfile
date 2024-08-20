pipeline {
    agent any
    environment{
        NETLIFY_SITE_ID = '4666b2d5-b388-4610-8464-415751c05ea7'
        //necessary as netlify checks for this site id with this var name, and gets to know about which site we want to deploy
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
        //necessary as netlify checks for this token with this var name.
    }
    stages {
        /*
        stage('build') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
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
        stage('tests'){
            parallel{
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
                        npx playwright test --reporter=html
                        npm install sharp
                        '''
                    }
                    
                }
                
            }
        }
        stage("Deploy"){
            agent{
                docker{
                    image 'iahs5/node-18-py3:1'
                    reuseNode true
                }
            }
            steps{
                sh '''
                npm install netlify-cli
                apk add --no-cache python3 py3-pip
                node_modules/.bin/netlify --version
                echo "Deploying to production: $NETLIFY_SITE_ID"
                node_modules/.bin/netlify status
                node_modules/.bin/netlify deploy --dir=build --prod
                '''
            }
        }
    }

}

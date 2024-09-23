pipeline {
    agent any
    environment{
        NETLIFY_SITE_ID = 'b8c6023c-3b4a-4e26-b9fa-6fac41515637'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
        REACT_APP_VERSION = "1.2.$BUILD_ID"
    }

    stages {
        stage('Docker'){
            sh'docker build -t myplaywright .'
        }
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Running via poll SCM"
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
        stage('Tests'){
            parallel{
                stage('Unit Test'){
            agent{
                    docker{
                        image 'node:18-alpine'
                        reuseNode true
                    }
                }
            steps{
                sh''' echo "test stage added"
                      #grep -r "index.html" /var/jenkins_home/workspace/learn-jenkins-app/build
                      npm test
                
                '''
            }
        }
        
            stage('E2E'){
                agent{
                        docker{
                            image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                            reuseNode true
                        }
                    }
                steps{
                    sh''' echo "End 2  End satge added"
                        npm install serve
                        node_modules/.bin/serve -s build & # will start in background
                        sleep 10
                        npx playwright test --reporter=html
                    
                    '''
                }
            }
        
        
    }

            }
        stage('Deploy Staging') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                   npm install netlify-cli
                   node_modules/.bin/netlify --version
                   echo "Deploying to Production Site ID : $NETLIFY_SITE_ID"
                   node_modules/.bin/netlify status
                   node_modules/.bin/netlify deploy --dir=build

                '''
            }
        } 
        stage('Deploy Prod') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                   npm install netlify-cli
                   node_modules/.bin/netlify --version
                   echo "Deploying to Production Site ID : $NETLIFY_SITE_ID"
                   node_modules/.bin/netlify status
                   node_modules/.bin/netlify deploy --dir=build --prod

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

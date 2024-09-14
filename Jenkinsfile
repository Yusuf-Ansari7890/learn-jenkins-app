pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
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
        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                   npm install netlify-cli -g
                   netlify

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

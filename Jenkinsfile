pipeline {
    agent any

    stages {
        /*stage('Build') {
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
        }*/
        stage('Test'){
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
                        image 'mcr.microsoft.com/playwright:v1.47.1-noble'
                        reuseNode true
                    }
                }
            steps{
                sh''' echo "End 2  End satge added"
                      npm install serve
                      serve -s build
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

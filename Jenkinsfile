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
        stage('Test'){
            agent{
                    docker{
                        image 'node:18-alpine'
                        reuseNode true
                    }
                }
            steps{
                sh''' echo "test stage added"
                      grep -r "index.html" /var/jenkins_home/workspace/learn-jenkins-app/build 
                      npm test
                
                '''
            }
        }
    }
}

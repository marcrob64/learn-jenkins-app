pipeline {
    agent any

    stages {
        /*
        stage('Build') {
            agent {
                docker {
                    image 'node:24-alpine'
                    reuseNode true
                }
            }
        steps {
                sh '''
                    node --version
                    npm --version
                    npm ci
                    npm run build
                ''' 
            }
        }
        */

        stage('Test') {
            agent {
                docker {
                    image 'node:24-alpine'
                    reuseNode true
                }
            }
            
            steps {
                sh '''
                    #test -f build/index.html
                    npm test
                '''
            }
        }
    
        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.61.1-noble'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    npm install serve
                    node_modules/.bin/serve -s build &
                    sleep 10
                    npx playwright test --reporter=html 
                '''
            }
        }
    }

    post {
        always {
            junit 'jest-results/junit.xml'
        }
    }
}
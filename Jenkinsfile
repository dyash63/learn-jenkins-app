pipeline {
    agent any

    stages {
        // this is a build phase
        /* 
        this is a block comment
        Hi Yash
        */
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
            stage('Test') {
                agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
                }
                steps{
                    echo 'testing stage'
                    sh ''' test -f build/index.html
                           npm test -a
                    '''
                }
            }
            stage('E2E') {
                agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.49.1-noble'
                    reuseNode true
                }
                }
                steps{
                    sh '''
                          npm install
                          npx playwright install chromium
                          npm install serve
                          node_modules/.bin/serve -s build &
                          sleep 10
                          npx playwright test --reporter=line
                    '''
                }
            }    
        }
        post {
        always{
            junit 'jest-results/junit.xml'
        }
        }
    }

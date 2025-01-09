pipeline {
    agent any
        environment{
            NETLIFY_SITE_ID = 'dcfa523a-2d32-44c9-9088-4e0abada9e74'
            NETLIFY_AUTH_TOKEN = credentials('netlify-token')
            CI_ENVIRONMENT_URL = 'https://didactic-enigma-g4r5wwqj56x6cw44-3000.app.github.dev'
        }
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
                    echo "small change"
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build --prod
                    echo "SITE ID : $NETLIFY_SITE_ID "
                '''
            }
        }
        stage('E2E PROD') {
                agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.49.1-noble'
                    reuseNode true
                }
                }
                environment{
                CI_ENVIRONMENT_URL = 'https://didactic-enigma-g4r5wwqj56x6cw44-3000.app.github.dev'
        }
                steps{
                    sh '''
                            npx playwright install chromium
                          npx playwright test --reporter=line
                    '''
                }
                 post {
                    always{
                    publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'playwright E2E Report', reportTitles: '', useWrapperFileDirectly: true])
        }
        }
            }
        stage('run tests'){
            parallel{
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
                 post {
                    always{
                      junit 'jest-results/junit.xml'
        }
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
                 post {
                    always{
                    publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'playwright Report', reportTitles: '', useWrapperFileDirectly: true])
        }
        }
            }

            }
        }

    
        }
       
    }

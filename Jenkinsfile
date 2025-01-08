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
        }
        post {
        always{
            junit 'test-results/junit.xml'
        }
        }
    }

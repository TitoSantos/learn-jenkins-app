pipeline {
    agent any

    stages {
        // This is stage to build the application
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
                    npm ci #This command is equal to npm install
                    npm run build
                    ls -la
                '''
            }
        }
         // This is stage to test the application
        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                echo 'Test stage'
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }

        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    #image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                echo 'Test stage'
                sh '''
                    npm install -g serve
                    serve -s build
                    npx playwright test
                '''
            }
        }        
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}


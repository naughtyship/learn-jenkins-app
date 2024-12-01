pipeline {
    agent any

    stages {
        // build stage
        stage('Build') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "checking the current directory"
                    ls -la
                    echo "Checking the node & npm version"
                    npm --version
                    node --version
                    npm ci
                    npm run build
                '''
            }
        }
        // build test
        stage('Test'){
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
            sh '''
                echo "Performing the test"
                test -f build/index.html
                ls ./build
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
            sh '''
                npm install -g serve
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

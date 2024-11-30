pipeline {
    agent any

    stages {
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
    }

    post{
        always{
            junit 'test-results/junit.xml'
        }
    }
}

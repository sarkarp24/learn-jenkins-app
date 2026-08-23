pipeline{
    agent any
    stages{
        /*
        stage('Workspace cleanup'){
            steps{
                echo 'Cleaning workspace...'
                cleanWs()
            }
        }
        */
        stage('Build'){
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                echo 'Building...'
                sh '''
                    ls -la
                    node --version
                    npm -version
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
                echo 'Testing...'
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }
        stage('Deploy'){
            steps{
                echo 'Deploying...'
            }
        }
    }
    post{
        always{
            junit 'jest-result/junit.xml'
        }
    }
}
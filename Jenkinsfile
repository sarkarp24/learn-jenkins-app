pipeline{
    agent any
    stages{
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
                    cleanWs()
                    ls -la
                    node --version
                    npm -version
                    npm ci
                    ls -la
                '''
            }
        }
        stage('Test'){
            steps{
                echo 'Testing...'
            }
        }
        stage('Deploy'){
            steps{
                echo 'Deploying...'
            }
        }
    }
}
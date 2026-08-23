pipeline{
    agent any
    stages{
        stage('Build'){
            agent{
                docker{
                    image 'node:alpine-18'
                    reuseNode true
                }
            }
            steps{
                echo 'Building...'
                sh '''
                    ls -la
                    node --version
                    npm -version
                    npm run build
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
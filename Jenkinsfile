pipeline{
    agent any
    environment{
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
        NETLIFY_SITE_ID = '4b92158c-9bc4-42bc-9e9a-1d962f0534f6'
    }
    stages{
        /*
        stage('Workspace cleanup'){
            steps{
                echo 'Cleaning workspace...'
                cleanWs()
            }
        }
        */
        stage('start time'){
            steps{
                echo 'Start time:'
                sh 'date'
                script{
                    env.START_TIME = sh(script: 'date +%s', returnStdout: true).trim()
                }
            }
        }
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
        stage('Unit Test'){
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
        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    npm install serve
                    ls -la node_modules/.bin
                    node_modules/.bin/serve -s build &
                    sleep 10
                    npx playwright test --reporter=html
                '''
            }
        }
        stage('Deploy'){
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps {
                script {
                    env.END_TIME = sh(script: 'date +%s', returnStdout: true).trim()
                }
                sh '''
                    echo 'Deploying to Netlify...'
                    npm install netlify-cli@20.1.1 node-jq
                    ls -la node_modules/.bin
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --prod --dir=build --json > netlify-deploy.json
                    echo 'Deployment completion time:'env.END_TIME - env.START_TIME
                '''
            }
        }
    }
    post{
        always{
            junit 'jest-results/junit.xml'
            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright HTML Report', reportTitles: '', useWrapperFileDirectly: true])
        }
    }
}
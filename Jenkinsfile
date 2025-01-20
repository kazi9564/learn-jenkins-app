pipeline {
    agent any

    stages {
        
        /*stage('Build') {
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
        */
        pipeline {
    agent any

    stages {
        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    npm install
                    npm test
                '''
            }
        }

        stage('E2E TEST') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.49.1-noble'
                    pull true
                    reuseNode true
                }
            }

            steps {
                sh '''
                    npm ci
                    npx playwright install
                    npx serve -s build &
                    sleep 10
                    npx playwright test --reporter=junit
                '''
            }
        }
    }

    post {
        always {
            junit '**/test-results/junit.xml'
            cleanWs()
        }
    }
}

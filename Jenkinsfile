pipeline {
    agent any

    stages {
        // This is a single line comment
        /*
        This 
        is
        multi
        line
        comment
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
            steps {
                sh '''
                    echo 'Test stage'
                    test -e build/index.html
                    npm test
                '''
            }   
        }

        stage('E2E') {
            agent {
                docker {
                    image 'docker pull mcr.microsoft.com/playwright:v1.50.0-noble'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install serve
                    node_modules/.bin/serve -s build &
                    sleep 10
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

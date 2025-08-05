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
                    image 'docker pull mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                    args '-u root'
                }
            }
            steps {
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

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
                ls -la
                node --version
                npm --version
                sudo npm ci
                npm run build
                '''
            }
        }
        stage('Test') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                    //args '-u root:root'
                }
            }
            steps {
                sh '''
                echo 'Test stage'
                test -f 'build/index.html'
                npm test
                '''
            }
        }

        stage('E2E') {
            agent{
                docker{
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                    //args '-u root:root'
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
        always{
            junit 'test-results/junit.xml'
        }
    }
}
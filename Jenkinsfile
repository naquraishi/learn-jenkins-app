pipeline {
    agent any

    stages {
        stage('Build') {
            agent{
                Docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                la -la
                node --version
                npm --version
                npm ci
                npm run build
                '''
            }
        }
    }
}
pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'd3f41068-0370-4050-a800-cfa425e61220'
    }

    stages {

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

        stage ('Test'){
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                test -f build/index.html
                npm test
                '''
            }
        }

        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                npm install netlify-cli
                node_modules/.bin/netlify --version
                echo "Deploying to poduction site id d3f41068-0370-4050-a800-cfa425e61220"
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
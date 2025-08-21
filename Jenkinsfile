pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '781bfd86-823a-44a0-b093-7ccbd8a5c11d'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages {
        stage('Build') {
            agent{
                docker {
                    image 'node:18-alpine'
                    
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
             agent{
                docker{
                    image 'node:18-alpine'
                    
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
            agent{
                docker{
                    image 'node:18-alpine'
                    
                }
            }
            steps {
                sh '''
                npm install netlify-cli 
                npx netlify --version
                echo "Deploying to production. Site ID: $NETLIFY_SITE_ID"
                npx netlify status
                echo "Deployed via Jenkins. Site ID : ${NETLIFY_SITE_ID}"
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

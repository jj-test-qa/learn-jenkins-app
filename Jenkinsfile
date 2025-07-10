pipeline {
    agent any

    stages {
        stage('Test'){
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    //args '-u root:root'
                }
            }
            steps {
                sh '''
                    #mkdir -p jest-results
                    #chown -R node:node jest-results
                    npm test
                '''
            }
        }
    
    
        stage('E2E'){
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
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
        always{
            junit 'jest-results/junit.xml'
        }
    }
}


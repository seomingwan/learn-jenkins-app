pipeline {
    agent {
        docker {
            image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
            reuseNode true
        }
    }

    stages {
        stage('Build') {

            steps {
                sh '''
                    ls -la
                    node -v
                    npm -v
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
        
        stage ('Test') {
            steps {
                echo 'Test stage'
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }

        stage ('E2E') {
            steps {
                sh '''
                    npm install serve
                    npx serve -s build & sleep 10
                    npx playwright test --reporter=html
                '''
            }
        }

        stage ('Deploy') {
            steps {
                sh '''
                    npm install netlify-cli@20.1.1
                    npx netlify -v
                    npx netlify deploy --dir=build --prod --auth=$NETLIFY_AUTH_TOKEN --site=$NETLIFY_SITE_ID
                '''
            }
        }
    }

    post {
        always {
            junit 'jest-results/junit.xml'
        }
    }
}

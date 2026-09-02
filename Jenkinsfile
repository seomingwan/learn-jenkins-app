pipeline {
    agent {
        docker {
            image 'node:18-alpine'
            args '-u root'
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
        
        stage('Test') {
            steps {
                echo 'Test stage'
                sh '''
                    test -f build/index.html
                    npm test -- --watchAll=false
                '''
            }
        }

        // stage('Test') {
        //     steps {
        //         // pytest 실행 시 결과를 xml 파일로 저장
        //         sh 'pytest --junitxml=test-result/junit.xml'
        //     }
        // }

    }

    post {
        always {
            junit allowEmptyResults: true, testResults: 'test-result/junit.xml'
        }
    }

}

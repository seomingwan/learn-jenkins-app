pipeline {
    agent any

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
                    npm test
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
            // 생성된 xml을 젠킨스 리포트로 등록 (동일하게 사용)
            junit 'test-result/junit.xml'
        }
    }

}

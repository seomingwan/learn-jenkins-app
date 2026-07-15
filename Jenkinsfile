pipeline {
    agent any

    stages {
        stage('1. GitHub Clone Test') {
            steps {
                echo '✅ GitHub로부터 소스코드를 성공적으로 가져왔습니다!'
                // 현재 폴더에 GitHub 파일들이 잘 들어왔는지 목록 출력
                sh 'ls -la' 
            }
        }

        stage('2. Simple System Check') {
            steps {
                echo '✅ Jenkins 서버 내부의 Java 및 AWS CLI 설치 여부를 테스트합니다.'
                sh 'java -version'
                sh 'aws --version'
            }
        }

        stage('3. Connection Success') {
            steps {
                echo '🚀 [성공] GitHub와 Jenkins 연동이 정상적으로 완료되었습니다!'
            }
        }
            
        stage('4. s3 Bucket Connection Test') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'my_aws', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                    sh '''
                        aws --version
                        aws s3 ls
                    '''
                }
            }
        }
            
        stage('5. s3 Bucket Upload Test') {
            
            environment {
                AWS_S3_BUCKET = 'unstuck-bucket'
            }

            steps {
                withCredentials([usernamePassword(credentialsId: 'my_aws', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                    sh '''
                        aws --version
                        echo "Hello S3!" > index.html
                        aws s3 cp index.html s3://$AWS_S3_BUCKET/index.html
                    '''
                }
            }
        }
    }

    post {
        always {
            echo '테스트 빌드가 완료되었습니다.'
        }
    }
}
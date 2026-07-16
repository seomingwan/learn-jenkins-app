pipeline {
     // 전역 에이전트를 사용하지 않음으로써 컨테이너 중첩 방지
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'ap-northeast-2' // AWS 기본 리전 설정
    }

    stages {

        // steps {
        //     withCredentials([usernamePassword(credentialsId: 'my_aws', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
        //         sh '''
        //             aws --version
        //             aws ecs register-task-definition --cli-input-json file://aws/task-definition-prod.json
        //         '''
        //     }
        // }

        /////////////////////////////////////////////////////////////////////////////////

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
                    '''
                }
            }
        }

    }

}
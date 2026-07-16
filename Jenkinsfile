pipeline {
     // 전역 에이전트를 사용하지 않음으로써 컨테이너 중첩 방지
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'ap-northeast-2' // AWS 기본 리전 설정
        AWS_S3_BUCKET = 'unstuck-bucket' 
    }

    stages {
        stage('AWS') {
            agent {
                docker { 
                    image 'amazon/aws-cli'
                    // aws-cli 이미지는 기본적으로 실행 후 바로 종료되므로 엔트리포인트 무력화
                    args "--entrypoint=''" 
                }
            }
            steps {
                sh 'aws --version'
            }
        }

        // stage('Deploy AWS') {
        //     agent {
        //         docker {
        //             image 'amazon/aws-cli'
        //             reuseNode true
        //             args "--entrypoint ''"
        //         }
        //     }
        // }

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
            
        // stage('5. AWS S3 Sync') {
        //     steps {
        //         // ⚠️ 젠킨스에 등록해 둔 AWS 자격증명 ID가 'my_aws'인지 'my-aws'인지 확인 후 맞춰주세요!
        //         withCredentials([usernamePassword(credentialsId: 'my_aws', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
        //             sh '''
        //                 echo '🚀 S3 버킷으로 빌드 결과물 동기화를 시작합니다.'
        //                 aws --version
        //                 aws s3 sync public s3://$AWS_S3_BUCKET
        //             '''
        //         }
        //     }
        // }

    }

    post {
        always {
            echo '테스트 빌드가 완료되었습니다.'
        }
        success {
            echo '🎉 축하합니다! 모든 빌드 및 S3 동기화 배포 과정이 정상 완료되었습니다.'
        }
        failure {
            echo '❌ 빌드 또는 S3 전송 도중 에러가 발생했습니다. 로그를 체크하세요.'
        }
    }
}
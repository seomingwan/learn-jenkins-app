pipeline {
     // 전역 에이전트를 사용하지 않음으로써 컨테이너 중첩 방지
    agent any

    environment {
        REACT_APP_VERSION = '1.0.${BUILD_ID}'
        APP_NAME = 'myjenkinsapp'
    }

    stages {

        stage('Build Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'my-jenkins', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                    sh '''
                        echo '🐳 도커 빌드 환경 변수를 보정하고 빌드를 시작합니다.'
                        
                        # 1. 꼬여있는 도커 호스트 환경 변수를 우분투 실제 소켓 경로로 강제 변경합니다.
                        export DOCKER_HOST="unix:///var/run/docker.sock"
                        export DOCKER_TLS_VERIFY=""
                        
                        # 2. 도커가 정상 작동하는지 확인합니다.
                        docker ps
                        
                        # 3. 이제 안전하게 도커 이미지를 빌드합니다.
                        docker build -t $APP_NAME:$REACT_APP_VERSION .
                    '''
                }
            }
        }

    }

}
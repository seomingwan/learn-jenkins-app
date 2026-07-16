pipeline {
     // 전역 에이전트를 사용하지 않음으로써 컨테이너 중첩 방지
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'ap-northeast-2' // AWS 기본 리전 설정
        AWS_ECS_CLUSTER = 'focused-frog-3wzw11' // ECS 클러스터 이름
        AWS_ECS_SERVICE_PROD = 'LearnJenkinsApp-Service-Prod ' // ECS 서비스 이름
        AWS_ECS_TD_PROD = 'LearnJenkinsApp-TaskDefinition-Prod' // ECS 태스크 정의 이름
    }

    stages {

        stage('Build Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'my_aws', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                    sh '''
                        echo '🐳 도커 빌드 환경 변수를 보정하고 빌드를 시작합니다.'
                        
                        # 1. 꼬여있는 도커 호스트 환경 변수를 우분투 실제 소켓 경로로 강제 변경합니다.
                        export DOCKER_HOST="unix:///var/run/docker.sock"
                        export DOCKER_TLS_VERIFY=""
                        
                        # 2. 도커가 정상 작동하는지 확인합니다.
                        docker ps
                        
                        # 3. 이제 안전하게 도커 이미지를 빌드합니다.
                        docker build -t myjenkinsapp .
                    '''
                }
            }
        }

        stage('Deploy to AWS'){
            steps {
                withCredentials([usernamePassword(credentialsId: 'my_aws', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                    sh '''
                        aws --version
                        jq --version
                        LATEST_TD_REVISION=$(aws ecs register-task-definition --cli-input-json file://aws/task-definition-prod.json | jq '.taskDefinition.revision')
                        echo $LATEST_TD_REVISION
                        aws ecs update-service --cluster $AWS_ECS_CLUSTER --service $AWS_ECS_SERVICE_PROD --task-definition $AWS_ECS_TD_PROD:$LATEST_TD_REVISION
                        aws ecs wait services-stable --cluster $AWS_ECS_CLUSTER --services $AWS_ECS_SERVICE_PROD
                    '''
                }
            }

        }

    }

}
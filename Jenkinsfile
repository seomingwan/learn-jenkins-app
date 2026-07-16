pipeline {
     // 전역 에이전트를 사용하지 않음으로써 컨테이너 중첩 방지
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'ap-northeast-2' // AWS 기본 리전 설정
    }

    stages {
        stage('Deploy to AWS'){
            steps {
                withCredentials([usernamePassword(credentialsId: 'my_aws', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                    sh '''
                        aws --version
                        yum install -y jq
                        LATEST_TD_REVISION=$(aws ecs register-task-definition --cli-input-json file://aws/task-definition-prod.json | jq '.taskDefinition.revision')
                        echo $LATEST_TD_REVISION
                        aws ecs update-service --cluster focused-frog-3wzw11 --service LearnJenkinsApp-Service-Prod --task-definition LearnJenkinsApp-TaskDefinition-Prod:2                 
                    '''
                }
            }

        }

    }

}
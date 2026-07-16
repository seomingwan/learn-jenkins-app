pipeline {
     // 전역 에이전트를 사용하지 않음으로써 컨테이너 중첩 방지
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'ap-northeast-2' // AWS 기본 리전 설정
    }

    stages {
        
        stage('Deploy to AWS'){
            
            agent {
                docker {
                    image 'amazon/aws-cli' // AWS CLI가 설치된 Docker 이미지 사용
                    reuseNode true // 노드 재사용 설정
                    args "--entrypoint=''"
                }
            }

            steps {
                withCredentials([usernamePassword(credentialsId: 'my_aws', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                    sh '''
                        aws --version
                        aws ecs register-task-definition --cli-input-json file://aws/task-definition-prod.json
                        aws ecs update-service --cluster focused-frog-3wzw11 --service LearnJenkinsApp-Service-Prod --task-definition LearnJenkinsApp-TaskDefinition-Prod:2                 
                    '''
                }
            }

        }

    }

}
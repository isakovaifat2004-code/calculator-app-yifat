pipeline {
    agent any
    
    environment {
        AWS_ACCOUNT_ID = "992382545251" // ראיתי שזה המספר שלך מהצילום מסך!
        REGION = "us-east-1"
        IMAGE_NAME = "calculator-app"
        ECR_URL = "${AWS_ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com/${IMAGE_NAME}"
    }

    stages {
        stage('Install Tools') {
            steps {
                script {
                    echo "Installing AWS CLI and Docker inside Jenkins..."
                    // התקנת הכלים שחסרים לג'נקינס
                    sh 'apt-get update && apt-get install -y awscli docker.io'
                }
            }
        }

        stage('Build Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    echo "Logging into ECR..."
                    // פקודת ההתחברות המתוקנת
                    sh "aws ecr get-login-password --region ${REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com"
                    
                    echo "Tagging and Pushing..."
                    sh "docker tag ${IMAGE_NAME}:latest ${ECR_URL}:latest"
                    sh "docker push ${ECR_URL}:latest"
                }
            }
        }
    }
}

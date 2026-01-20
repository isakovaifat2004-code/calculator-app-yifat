pipeline {
    agent any
    
    environment {
        // !!! כאן את חייבת לשים את מספר החשבון שלך (12 ספרות) !!!
        AWS_ACCOUNT_ID = "992382545251" 
        REGION = "us-east-1"
        IMAGE_NAME = "calculator-app"
        ECR_URL = "${AWS_ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com/${IMAGE_NAME}"
    }

    stages {
        stage('Build Image') {
            steps {
                // הבנייה שכבר עובדת לך
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    echo "Logging into ECR..."
                    // התחברות ל-ECR (עובד בזכות ה-Role ששמנו לשרת)
                    sh "aws ecr get-login-password --region ${REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com"
                    
                    echo "Tagging and Pushing..."
                    sh "docker tag ${IMAGE_NAME}:latest ${ECR_URL}:latest"
                    sh "docker push ${ECR_URL}:latest"
                }
            }
        }
    }
}

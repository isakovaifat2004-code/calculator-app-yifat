pipeline {
    agent any

    environment {
        // !!! תשני את המספר 123456789012 למספר החשבון שלך מאמזון !!!
        ECR_URL = "123456789012.dkr.ecr.us-east-1.amazonaws.com/calculator-app"
        REGION = "us-east-1"
    }

    stages {
        stage('Build Image') {
            steps {
                script {
                    echo "Building Docker Image..."
                    // בניית האימג' מה-Dockerfile שהעלית
                    sh "docker build -t calculator-app ."
                }
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    echo "Pushing to ECR..."
                    // התחברות לאמזון (עובד בזכות ה-Role שנתנו ל-EC2)
                    sh "aws ecr get-login-password --region ${REGION} | docker login --username AWS --password-stdin ${ECR_URL}"
                    
                    // תיוג ודחיפה
                    sh "docker tag calculator-app:latest ${ECR_URL}:latest"
                    sh "docker push ${ECR_URL}:latest"
                }
            }
        }
    }
}

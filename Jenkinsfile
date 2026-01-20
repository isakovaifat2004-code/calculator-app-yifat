pipeline {
    agent any 
    
    environment {
        // כאן את שמה את ה-URI של ה-ECR שפתחת באמזון
        ECR_REGISTRY = "123456789012.dkr.ecr.us-east-1.amazonaws.com/calculator-app"
    }

    stages {
        stage('Build & Test') {
            steps {
                echo 'Building and Testing...'
                // כאן ג'נקינס בונה את הקונטיינר ומריץ בדיקות
                sh "docker build -t calculator-test ."
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    // התחברות לאמזון (זה יעבוד כי שמנו Role לשרת)
                    sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ECR_REGISTRY}"
                    // דחיפה של האימג'
                    sh "docker tag calculator-test ${ECR_REGISTRY}:latest"
                    sh "docker push ${ECR_REGISTRY}:latest"
                }
            }
        }
    }
}

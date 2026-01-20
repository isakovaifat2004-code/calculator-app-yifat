pipeline {
    agent any
    
    stages {
        stage('Install Docker inside Jenkins') {
            steps {
                script {
                    // הפקודה הזו מוודאת שבתוך ג'נקינס מותקן הלקוח של דוקר
                    sh 'apt-get update && apt-get install -y docker.io'
                }
            }
        }
        stage('Build Image') {
            steps {
                sh 'docker build -t calculator-app .'
            }
        }
        stage('Push to ECR') {
            steps {
                echo "Ready for ECR push!"
                // כאן יבוא ה-Push בהמשך
            }
        }
    }
}

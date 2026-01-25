pipeline {
    agent any 
    environment {
        AWS_ACCOUNT_ID = "992382545251" // המספר מהתמונה שלך
        REGION = "us-east-1"
        IMAGE_NAME = "calculator-app-yifat" // השם מה-ECR שלך
        ECR_URL = "${AWS_ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com/${IMAGE_NAME}"
        PROD_SERVER_IP = "ה-IP-של-השרת-פרודקשן-החדש-שלך"
    }
    stages {
        stage('Test') {
            steps {
                // הרצת הטסטים כפי שמופיע ב-README שלך
                sh 'python3 -m unittest discover -s tests -v'
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
                    sh "aws ecr get-login-password --region ${REGION} | docker login --username AWS --password-stdin ${ECR_URL}"
                    // לוגיקת תיוג לפי דרישות המורה: לא רק latest!
                    def tag = (env.BRANCH_NAME == 'master') ? "latest" : "pr-${env.CHANGE_ID}-${env.BUILD_NUMBER}"
                    sh "docker tag ${IMAGE_NAME} ${ECR_URL}:${tag}"
                    sh "docker push ${ECR_URL}:${tag}"
                }
            }
        }
        stage('Deploy') {
            when { branch 'master' } // דיפלוי קורה רק במיזוג למאסטר
            steps {
                sshagent(credentials: ['prod-ssh-key']) {
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@${PROD_SERVER_IP} 'docker stop calc-app || true && docker rm calc-app || true'"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@${PROD_SERVER_IP} 'aws ecr get-login-password --region ${REGION} | docker login --username AWS --password-stdin ${ECR_URL}'"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@${PROD_SERVER_IP} 'docker run -d -p 5000:5000 --name calc-app ${ECR_URL}:latest'"
                }
            }
        }
        stage('Health Check') {
            when { branch 'master' }
            steps {
                // בדיקת ה-API כפי שמופיע בקוד שלך
                sh "sleep 5 && curl -f http://${PROD_SERVER_IP}:5000/health"
            }
        }
    }
}

pipeline {

    agent any

    stages {

         stage('Init') {

            steps {

                sh '''
                docker network create jenk-network || echo "Network Already Exist"
                docker stop flask-app || echo "flask-app Not Running"
                docker stop nginx || echo "nginx Not Running"
                docker rm flask-app || echo "flask-app Not Running"
                docker rm nginx || echo "nginx Not Running"
                '''
             }
             }

        stage('Build') {

            steps {

                sh '''
                docker build -t flask-jenk .
                docker build -t nginx-jenk ./nginx
                '''
            }

        }

        stage('Deploy') {

            steps {

                sh '''
                docker run -d --name flask-app --network jenk-network flask-jenk       
                docker run -d -p 80:80 --name Piers --network jenk-network nginx-jenk
                '''
            }

        }
stage('Clean Up') {

            steps {

                sh '''
                docker system prune -f
                '''  
                            }
       

    }








}
}
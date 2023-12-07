pipeline {

    agent any

    stages {

         stage('Init') {

            steps {

                sh '''
                docker network create jenk-network || echo "Network Already Exist"
                docker stop flask-app || echo "flask-app Not Running"
                docker stop piers || echo "piers Not Running"
                docker rm flask-app || echo "flask-app Not Running"
                docker rm piers || echo "piers Not Running"
                                '''
             }
             }

        stage('Build') {

            steps {

                sh '''
                docker build -t prenticed103/flask-jenk:latest -t prenticed103/flask-jenk:v${BUILD_NUMBER} .
                docker build -t prenticed103/nginx-jenk:latest -t prenticed103/nginx-jenk:v${BUILD_NUMBER} ./nginx
                '''
            }

        }

  stage('Push') {

            steps {
                sh '''
                docker push prenticed103/flask-jenk:latest
                docker push prenticed103/flask-jenk:v$:{BUILD_NUMBER}
                docker push prenticed103/nginx-jenk:latest
                docker push prenticed103/nginx-jenk:v$:{BUILD_NUMBER}
                '''
            }
  }
        stage('Deploy') {

            steps {

                sh '''
                docker run -d --name flask-app --network jenk-network prenticed103/flask-jenk       
                docker run -d -p 80:80 --name piers --network jenk-network prenticed103/nginx-jenk
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

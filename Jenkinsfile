pipeline {

    agent any

    stages {
       
        stage('Build') {

            steps {

                sh '''
                docker build -t prenticed103/flask-jenk:latest -t prenticed103/flask-jenk:v${BUILD_NUMBER} .
                   '''
            }

        }

  stage('Push') {

            steps {
                sh '''
                docker push prenticed103/flask-jenk:latest
                docker push prenticed103/flask-jenk:v${BUILD_NUMBER}
                  '''
            }
  }
        stage('Deploy') {

            steps {

                sh '''
                kubectl apply -f ./kubernetes
                kubectl rollout restart deployment/flask-deployment
                kubectl rollout restart deployment/nginx-deployment
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

pipeline {
    agent any
    
    stages {
        stage('get repo') {
            steps {
                git url: 'https://github.com/ShroukSamir11/nodejs-app'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t shrouksamir11/task-img .'

            }
        }
       
        stage('Push Image')
        {
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    sh 'echo $pass | docker login -u $user --password-stdin'
                    sh 'docker push $user/task-img'
                }
                
            }
        }
        stage ('Create deployment and service'){
            steps{
                sh 'kubectl apply -f deployment.yml'
                sh 'kubectl apply -f nodeport-svc.yml'
                sh 'docker logout'
            }
        }
    }
   
}

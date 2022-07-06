pipeline{
    agent any
    stages{
        stage('Git Checkout') {
        steps {
                checkout scm
            }
        }

        stage('Build'){
            steps{
                sh 'sudo docker build -t node-app .'
                sh 'sudo docker tag node-app:latest 022536480424.dkr.ecr.us-east-1.amazonaws.com/node-app:latest'
                sh 'sudo docker push 022536480424.dkr.ecr.us-east-1.amazonaws.com/node-app:latest'
            }
        }
    }
}
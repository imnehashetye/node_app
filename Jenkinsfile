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
                sh 'cd node_app && sudo docker build -t node_app .'
                sh 'sudo docker tag node_app:latest 022536480424.dkr.ecr.us-east-1.amazonaws.com/node_app:latest'
                sh 'sudo docker push 022536480424.dkr.ecr.us-east-1.amazonaws.com/node_app:latest'
            }
        }
    }
}
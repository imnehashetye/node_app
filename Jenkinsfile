pipeline{
    agent {
        label 'worker'
    }
    stages{
        stage('Git Checkout') {
        steps {
                checkout scm
            }
        }

        stage('Build'){
            steps{
                sh 'docker build -t node_app .'
                sh 'docker tag node_app:latest 022536480424.dkr.ecr.us-east-1.amazonaws.com/node_app:latest'
                sh 'docker push 022536480424.dkr.ecr.us-east-1.amazonaws.com/node_app:latest'
            }
        }
    }
}
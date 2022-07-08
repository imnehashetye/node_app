pipeline{
    agent {
        label 'worker'
    }
    options {
        buildDiscarder(logRotator(numToKeepStr: '2'))
        disableConcurrentBuilds()
        timeout(time: 5, unit: 'MINUTES')
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

        stage('Deploy'){
            steps{
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@2ndserver && cd /home/ubuntu/ && sudo touch test-file && docker pull 022536480424.dkr.ecr.us-east-1.amazonaws.com/node_app:latest'
                sh 'docker ps'
                sh 'docker stop 022536480424.dkr.ecr.us-east-1.amazonaws.com/node_app:latest'
                sh 'docker run -p8080:8080 022536480424.dkr.ecr.us-east-1.amazonaws.com/node_app:latest'
            }
        }
    }
    post {
        always {
            deleteDir()
            sh 'docker rmi 022536480424.dkr.ecr.us-east-1.amazonaws.com/node_app:latest'
        }

    }
}
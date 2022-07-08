pipeline{
    agent {
        label 'worker'
    }
    options {
        buildDiscarder(logRotator(numToKeepStr: '2'))
        disableConcurrentBuilds()
        timeout(time: 3, unit: 'MINUTES')
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
            def dockerRun = 'docker pull 022536480424.dkr.ecr.us-east-1.amazonaws.com/node_app:latest'
            // sh'ssh -i ~/session.pem -o StrictHostKeyChecking=no ubuntu@ec2-44-208-23-240.compute-1.amazonaws.com ${dockerRun}'
            steps{
                // def dockerRun = 'docker pull 022536480424.dkr.ecr.us-east-1.amazonaws.com/node_app:latest'
                sh'ssh -i ~/session.pem -o StrictHostKeyChecking=no ubuntu@ec2-44-208-23-240.compute-1.amazonaws.com ${dockerRun}'
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
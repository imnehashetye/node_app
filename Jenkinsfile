pipeline{
    agent {
        label 'worker'
    }
    options {
        buildDiscarder(logRotator(numToKeepStr: '2'))
        disableConcurrentBuilds()
        timeout(time: 10, unit: 'MINUTES')
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
            // def dockerRun = 'docker pull 022536480424.dkr.ecr.us-east-1.amazonaws.com/node_app:latest'
            // sh'ssh -i ~/session.pem -o StrictHostKeyChecking=no ubuntu@ec2-44-208-23-240.compute-1.amazonaws.com ${dockerRun}'
            steps{
                script {
                    sh 'ssh -i ~/session.pem -o StrictHostKeyChecking=no ubuntu@ec2-18-212-64-133.compute-1.amazonaws.com'
                    // sh'''ssh -tt -i ~/session.pem -o StrictHostKeyChecking=no ubuntu@ec2-18-212-64-133.compute-1.amazonaws.com && aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 022536480424.dkr.ecr.us-east-1.amazonaws.com && docker pull 022536480424.dkr.ecr.us-east-1.amazonaws.com/node_app:latest && docker ps'''
                    sh 'ssh ubuntu@ec2-18-212-64-133.compute-1.amazonaws.com aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 022536480424.dkr.ecr.us-east-1.amazonaws.com'
                    sh 'ssh ubuntu@ec2-18-212-64-133.compute-1.amazonaws.com docker pull 022536480424.dkr.ecr.us-east-1.amazonaws.com/node_app:latest'
                    sh 'ssh ubuntu@ec2-18-212-64-133.compute-1.amazonaws.com docker images'
                    sh 'ssh ubuntu@ec2-18-212-64-133.compute-1.amazonaws.com docker ps'
                    sh 'ssh ubuntu@ec2-18-212-64-133.compute-1.amazonaws.com docker kill $(docker ps -q)'
                    sh 'ssh ubuntu@ec2-18-212-64-133.compute-1.amazonaws.com docker run -d -p8080:8080 022536480424.dkr.ecr.us-east-1.amazonaws.com/node_app:latest'
                }
                
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
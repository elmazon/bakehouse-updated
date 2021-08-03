pipeline{
    agent any
    stages{
        stage('docker build'){
            steps{
                sh "sudo docker build . -t ahmedelmazon/bakehouse"
            }
        }
        stage('dockerhub push'){
            steps{
                sh "sudo docker login -u ${username}  -p ${password}"
                sh "sudo docker push  ahmedelmazon/bakehouse"
            }
        }
    }
}

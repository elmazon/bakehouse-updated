pipeline{
    agent any
    stages{
        stage('docker build'){
            steps{
                sh "docker build . -t ahmedelmazon/bakehouse"
            }
        }
        stage('dockerhub push'){
            steps{
                sh "docker login -u ${username}  -p ${password}"
                sh "docker push  ahmedelmazon/bakehouse"
            }
        }
    }
}


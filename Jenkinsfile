pipeline{
    agent any
    stages{
        stage('pull image'){
            steps{
                sh "docker login -u ${username} -p ${password}"
                sh "docker pull  ahmedelmazon/bakehouse"
            }
        }
        stage('apply dep minikube'){
            steps{
                sh "kubectl  apply -f deployment.yaml"
                sh "kubectl  apply -f service.yaml"
            }
        }
    }
}
pipeline{
    agent any
    stages{
        stage('pull image'){
            steps{
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
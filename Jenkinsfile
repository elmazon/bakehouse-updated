pipeline{
    agent any
    parameters {
        choice(name: 'BRANCH', choices: ['dev', 'test', 'release', 'prod'])
    } 
    stages{
        stage('SCM'){
            steps{
                sh "git fetch"  
                sh "git checkout -b release"              
                //sh "git checkout ${params.BRANCH}"
                checkout scm
            }
        }        
        stage('docker build'){
            steps{
                checkout scm
                script{
                    if (params.BRANCH == 'release'){
                        withCredentials([usernameColonPassword(credentialsId: 'docker-pass', variable: 'docker-password')]) {
                            sh "docker login -u ahmedelmazon -p ${docker-password}"
                        }
                        sh "docker build . -t ahmedelmazon/bakehouse"
                    }
                }
            }
        }
        stage('dockerhub push'){
            steps{
                script{
                    if (params.BRANCH == 'release'){
                        sh "docker push  ahmedelmazon/bakehouse"
                    }
                }
            }
        }
        stage('deployment'){
            steps {
                script{
                    if (params.BRANCH == 'dev'){
                        sh "kubectl create -f deployment.yaml"
                        sh "kubectl create -f service.yaml"
                        sh "checkout scm ${params.BRANCH}"
                    }
                    else if (params.BRANCH == 'test'){
                        sh "kubectl create -f deployment.yaml"
                        sh "kubectl create -f service.yaml"
                        sh "checkout scm ${params.BRANCH}"
                    }
                    else (params.BRANCH == 'prod'){
                        sh "kubectl create -f deployment.yaml"
                        sh "kubectl create -f service.yaml"
                        sh "checkout scm ${params.BRANCH}"
                    }                        
                }
            }
        }
    }
}
pipeline{
    agent any
    parameters {
        choice(name: 'BRANCH', choices: ['release', 'dev', 'test', 'prod'])
    } 
    stages{
        stage('SCM'){
            steps{             
                sh "git checkout ${params.BRANCH}"
                checkout scm
            }
        }     
        stage('docker build'){
            steps{
                checkout scm
                sh "git checkout -b ${params.BRANCH}" 
                script{
                    if (params.BRANCH == 'release'){
                        sh "docker build . -t ahmedelmazon/bakehouse"
                        withCredentials(usernameColonPassword([credentialsId: 'docker-password', variable: 'docker-pass'])) {
                            sh "docker login -u ahmedelmazon -p ${docker-pass}"       
                        }
                        
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
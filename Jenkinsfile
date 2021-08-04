pipeline{
    agent any
    parameters {
        choice(name: 'BRANCH', choices: ['release', 'dev', 'test', 'prod'])
    } 
    stages{
        stage('SCM'){
            steps{
                sh "git checkout ${params.BRANCH}"
            }
        }
        stage('new_stage') {
            steps {
                 script {
                    withCredentials([usernamePassword(credentialsId: 'docker_password', passwordVariable: 'password', usernameVariable: 'user')]) {
                        sh 'docker login -u ahmedelmazon -p ${password}'
                    }           
                    if  (params.BRANCH == 'release') 
                            {
                                sh 'docker build . -t ahmedelmazon/bakehouse'
                                sh 'docker push ahmedelmazon/bakehouse'
                            
                            }
                     else if  (params.BRANCH == 'prod') 
                            {
                                sh 'docker pull ahmedelmazon/bakehouse'
                                sh 'kubectl apply -f deployment.yaml'
                                sh 'kubectl apply -f service.yaml'
                             }
                     else if  (params.BRANCH == 'dev') 
                            {
                                sh 'docker pull ahmedelmazon/bakehouse'
                                sh 'kubectl apply -f deployment.yaml'
                                sh 'kubectl apply -f service.yaml'

                            }
                     else (params.BRANCH == 'test') 
                            {
                                sh 'docker pull ahmedelmazon/bakehouse'
                                sh 'kubectl apply -f deployment.yaml'
                                sh 'kubectl apply -f service.yaml'

                            }
                   
                 }
            }        
        }
    }
}
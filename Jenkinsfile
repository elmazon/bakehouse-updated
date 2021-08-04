pipeline{
    agent any
    parameters {
        choice(name: 'BRANCH', choices: ['release', 'dev', 'test', 'prod'])
    } 
    stages{
        stage('new_stage') {
            steps {
                 sh 'git checkout ${params.BRANCH}'
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
                                sh "git checkout ${params.BRANCH}"
                                sh 'docker pull ahmedelmazon/bakehouse'
                                sh 'kubectl apply -f deployment.yaml'
                                sh 'kubectl apply -f service.yaml'
                             }
                     else if  (params.BRANCH == 'dev') 
                            {
                                sh "git checkout ${params.BRANCH}"
                                sh 'docker pull ahmedelmazon/bakehouse'
                                sh 'kubectl apply -f deployment.yaml'
                                sh 'kubectl apply -f service.yaml'

                            }
                     else (params.BRANCH == 'test') 
                            {
                                sh "git checkout ${params.BRANCH}"
                                sh 'docker pull ahmedelmazon/bakehouse'
                                sh 'kubectl apply -f deployment.yaml'
                                sh 'kubectl apply -f service.yaml'

                            }
                   
                 }
            }        
        }
    }
}
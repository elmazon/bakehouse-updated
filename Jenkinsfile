pipeline{
    agent any
    parameters {
        choice(name: 'BRANCH', choices: ['release', 'dev', 'test', 'prod'])
    } 
    stages{
        stage('new_stage') {
            steps {
                 script {
                    withCredentials(usernameColonPassword([credentialsId: 'docker-password', variable: 'docker-pass'])) {
                        sh 'docker login -u ahmedelmazon -p ${docker-pass}'       
                    }
                    if  (params.BRANCH == 'release') 
                            {
                                sh "git checkout ${params.BRANCH}"
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
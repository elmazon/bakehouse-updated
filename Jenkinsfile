pipeline{
    agent any
    parameters {
        choice(name: 'BRANCH', choices: ['release', 'dev', 'test', 'prod'])
    } 
    stages{
        stage('SCM'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/release']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/elmazon/bakehouse-updated']]])
                //sh "git checkout ${params.BRANCH}"
                //checkout SCM
            }
        }
        stage('new_stage') {
            steps {
                 script {
                    withCredentials([usernamePassword(credentialsId: 'docker_password', passwordVariable: 'password', usernameVariable: 'user')]) {
                        sh 'docker login -u ahmedelmazon -p ${password}'
                    }           
                    if  (params.BRANCH == 'release'){
                                checkout([$class: 'GitSCM', branches: [[name: '${params.BRANCH}']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/elmazon/bakehouse']]])
                                sh 'docker build . -t ahmedelmazon/bakehouse'
                                sh 'docker push ahmedelmazon/bakehouse'
                        }
                    else if  (params.BRANCH == 'prod') 
                        {
                            checkout([$class: 'GitSCM', branches: [[name: '*/${params.BRANCH}']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/elmazon/bakehouse']]])
                            sh 'docker pull ahmedelmazon/bakehouse'
                            sh 'kubectl apply -f deployment.yaml'
                            sh 'kubectl apply -f service.yaml'
                        }
                    else if  (params.BRANCH == 'dev') 
                        {
                            checkout([$class: 'GitSCM', branches: [[name: '*/dev']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/elmazon/bakehouse']]])
                            sh 'docker pull ahmedelmazon/bakehouse'
                            sh 'kubectl apply -f deployment.yaml'
                            sh 'kubectl apply -f service.yaml'

                        }
                    else (params.BRANCH == 'test') 
                        {
                            checkout([$class: 'GitSCM', branches: [[name: '*/${params.BRANCH}']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/elmazon/bakehouse']]])
                            sh 'docker pull ahmedelmazon/bakehouse'
                            sh 'kubectl apply -f deployment.yaml'
                            sh 'kubectl apply -f service.yaml'

                        }
                
                 }
            }        
        }
    }
}
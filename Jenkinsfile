pipeline {
    agent any
  
      stages {
        stage('Deploy to stage') {
            steps {
                sshagent (['ansible-key']) {
                     sh 'ssh -t -t ubuntu@54.186.7.33 -o strictHostKeyChecking=no "cd ~/ansible && ansible-playbook -i ~/inventory ~/ansible/staging.yml"'
                }       
            }  
        }
          stage('Slack notification') {
              steps {
                  slackSend channel: '3rd-april-sock-shop-microservices-kubernetes--project-us-team', message: 'App was applied to staging and needs approval to be deployed to production'
              }
          }
        stage('Request for approval') {
            steps {
                timeout(activity: true, time: 5){
                      input message: 'Deployment to production needs approval', submitter: 'admin'
                } 
            }      
        }
        stage('Deploy to prod') {
            steps {
                sshagent (['ansible-key']) {
                     sh 'ssh -t -t ubuntu@54.186.7.33 -o strictHostKeyChecking=no "cd ~/ansible && ansible-playbook -i ~/inventory ~/ansible/prod.yml"'
                }       
            }  
        }
    }
}

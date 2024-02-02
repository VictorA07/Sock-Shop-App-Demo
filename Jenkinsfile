pipeline {
    agent any
  
      stages {
        stage('Deploy to stage') {
            steps {
                sshagent (['ansible-key']) {
                     sh 'ssh -t -t ubuntu@10.0.4.201 -o strictHostKeyChecking=no "ansible-playbook ~/playbook/stage.yml"'
                }       
            }  
        }
          stage('Slack notification') {
              steps {
                  slackSend channel: '8-january-sock-shop-kubernetes-project-using-ansible-eu-team-1', message: 'App was applied to staging and needs approval to be deployed to production'
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
                     sh 'ssh -t -t ubuntu@10.0.4.201 -o strictHostKeyChecking=no "ansible-playbook ~/playbook/prod.yml"'
                }       
            }  
        }
    }
}

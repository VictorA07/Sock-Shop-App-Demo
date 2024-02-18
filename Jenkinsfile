pipeline {
  agent any
  stages {
    stage ('deploying to stage environment'){
      steps {
        sshagent(['ansible-key']) {
          sh 'ssh -t -t ubuntu@10.0.4.223 -o StrictHostKeyChecking=no "ansible-playbook /home/ubuntu/playbooks/stage.yml"'
        }
      }
    }
    stage ('slack notification'){
      steps {
        slackSend channel: '13-november-sock-shop-kubernetes-project-using-ansible-eu-team-1', message: 'New notification', tokenCredentialId: 'slack-cred'
      }
    }
    stage ('prompt for approval'){
      steps {
        timeout(activity: true, time: 5) {
          input message: 'Review before approval', submitter: 'admin'
        }
      }
    }
    stage ('deploying to prod environment'){
      steps {
        sshagent(['ansible-key']) {
          sh 'ssh -t -t ubuntu@10.0.4.223 -o StrictHostKeyChecking=no "ansible-playbook /home/ubuntu/playbooks/prod.yml"'
        }
      }
    }
  }
}

pipeline {
  agent any 

  stages {
    stage('Daily Compliance Run') {
      steps{
        echo 'Running a compliance scan with inspec....'
          script{
            def remote = [:]
            remote.name = "controlnode"
            remote.host = "34.135.35.220"
            remote.allowAnyHosts = true

            withCredentials([sshUserPrivateKey(credentialsId: 'sshUser', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'userName')]) {
                remote.user = userName
                remote.identityFile = identity
                stage("Enforce Compliance with Ansible") {
                  sshCommand remote: remote, sudo: true, command: 'echo "cd /home/ccedeno/secops/ansible && git pull origin"'
                  sshCommand remote: remote, sudo: true, command: 'echo "cd /root/secops/ansible && ansible-playbook compliance.yaml"'
              }
                stage("Scan with InSpec") {
                  sshCommand remote: remote, sudo: true, command: 'inspec exec --no-distinct-exit /home/ccedeno/linux-baseline/'
              }
            }
          }
       }
    }
  }
}

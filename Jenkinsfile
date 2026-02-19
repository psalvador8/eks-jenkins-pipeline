pipeline {   
    agent any
    stages {
        stage("copy files to ansible server") {
            steps {
                script{
                    echo "copy all neccessarry files to ansible conrol node"
                    sshagent(['ansible-server-key']) {
                       sh "scp -o StrictHostKeyChecking=no ansible/* root@138.68.74.61:/root"

                       withCredentials([sshUserPrivateKey(credentialsId: 'ec2-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]) {
                        sh 'scp $keyfile root@138.68.74.61:/root/ssh-key.pem'
                       }
                    }
                }
            }
        } 
        stage ("execute ansible playbook") {
            steps {
               script {
                    echo "calling ansible playbook to configure ec2 intances"
                    def remote = [:]
                    remote.name = "ansible-server"
                    remote.host = "138.68.74.61"
                    remote.allowAnyHosts = true

                    withCredentials([sshUserPrivateKey(credentialsId: 'ansible-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]) {
                        remote.user = user
                        remote.identityFile = keyfile
                        sshCommand remote: remote, command: "ls -l"
                    }              
                }
            }
        }             
    }
} 

pipeline {
    agent any
    
    tools {
        ansible 'ansible'  // Use the name from Global Tool Configuration
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', 
                    credentialsId: 'github-token-id', 
                    url: 'https://github.com/chmehervinay1993-maker/jenkinrepo.git'
            }
        }

        stage('Deploy with Ansible') {
            steps {
                withCredentials([sshUserPrivateKey(
                    credentialsId: 'ssh-id', 
                    keyFileVariable: 'SSH_KEY'
                )]) {
                    echo 'Deploying to multiple web servers using Ansible...'
                    
                    // Copy SSH key to temporary location for Ansible
                    sh 'cp ${SSH_KEY} /tmp/ssh_key'
                    sh 'chmod 600 /tmp/ssh_key'
                    
                    // Execute Ansible playbook
                    ansiblePlaybook(
                        playbook: 'deploy.yml',
                        inventory: 'inventory/hosts.ini',
                        colorized: true
                    )
                    
                    // Clean up
                    sh 'rm -f /tmp/ssh_key'
                }
            }
        }
    }
}

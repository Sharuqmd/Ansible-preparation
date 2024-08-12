pipeline {
    agent any
    environment {
        ANSIBLE_SERVER_IP = 'ansible-server-ip'
        ANSIBLE_USER = 'jenkins' // Ensure this matches the SSH username configured in Jenkins
        SSH_CREDENTIALS_ID = 'your-ssh-credentials-id'
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-repo/your-playbook-repo.git'
            }
        }
        stage('Copy Playbooks and Inventory to Ansible Server') {
            steps {
                sshagent([SSH_CREDENTIALS_ID]) {
                    // Copy both prometheus.yaml and inventory.yaml
                    sh "scp -o StrictHostKeyChecking=no prometheus.yaml inventory.yaml ${ANSIBLE_USER}@${ANSIBLE_SERVER_IP}:/home/ubuntu"
                }
            }
        }
        stage('Execute Playbook on Ansible Server') {
            steps {
                sshagent([SSH_CREDENTIALS_ID]) {
                    // Execute the playbook with the inventory file
                    sh "ssh -o StrictHostKeyChecking=no ${ANSIBLE_USER}@${ANSIBLE_SERVER_IP} 'ansible-playbook -i /home/ubuntu/inventory.yaml /home/ubuntu/prometheus.yaml'"
                }
            }
        }
    }
}

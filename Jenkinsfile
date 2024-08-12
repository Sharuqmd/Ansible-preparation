pipeline {
    agent any
    environment {
        ANSIBLE_SERVER_IP = '13.235.99.115'
        ANSIBLE_USER = 'jenkins' 
        SSH_CREDENTIALS_ID = 'jenkins'
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Sharuqmd/Ansible-preparation.git'
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

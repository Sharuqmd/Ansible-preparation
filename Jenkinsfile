pipeline {
    agent any
    environment {
        ANSIBLE_SERVER_IP = '13.235.99.115'
        ANSIBLE_USER = 'jenkins' 
        SSH_CREDENTIALS_ID = 'your-ssh-credentials-id'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Sharuqmd/Ansible-preparation.git'
            }
        }
        stage('Prepare Directory') {
            steps {
                // Create a directory on the Jenkins server to hold the files
                sh 'mkdir -p /home/jenkins/ansible_playbooks'
            }
        }
        stage('Copy Playbooks and Inventory to Directory') {
            steps {
                // Copy the playbooks and inventory file to the created directory
                sh 'cp prometheus.yaml inventory.yaml /home/jenkins/ansible_playbooks/'
            }
        }
        stage('Execute Playbook on Ansible Server') {
            steps {
                sshagent([SSH_CREDENTIALS_ID]) {
                    // Execute the playbook with the inventory file on the Jenkins server
                    sh "ssh -o StrictHostKeyChecking=no ${ANSIBLE_USER}@${ANSIBLE_SERVER_IP} 'ansible-playbook -i /home/jenkins/ansible_playbooks/inventory.yaml /home/jenkins/ansible_playbooks/prometheus.yaml'"
                }
            }
        }
    }
}

pipeline {
    agent any
    environment {
        ANSIBLE_SERVER_IP = 'ansible-server-ip'
        ANSIBLE_USER = 'user'
        SSH_CREDENTIALS_ID = 'your-ssh-credentials-id'
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-repo/your-playbook-repo.git'
            }
        }
        stage('Copy Playbook to Ansible Server') {
            steps {
                sshagent([SSH_CREDENTIALS_ID]) {
                    sh "scp -o StrictHostKeyChecking=no prometheus.yaml ${ANSIBLE_USER}@${ANSIBLE_SERVER_IP}:/home/ubuntu"
                }
            }
        }
        stage('Execute Playbook on Ansible Server') {
            steps {
                sshagent([SSH_CREDENTIALS_ID]) {
                    sh "ssh -o StrictHostKeyChecking=no ${ANSIBLE_USER}@${ANSIBLE_SERVER_IP} 'ansible-playbook /home/ubuntu/prometheus.yaml'"
                }
            }
        }
    }
}

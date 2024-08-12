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
                git branch: 'main', url: 'https://github.com/Sharuqmd/Ansible-preparation.git'
            }
        }
        stage('Prepare Directory in Workspace') {
            steps {
                // Create a directory inside the Jenkins workspace to store playbooks and inventory
                sh 'mkdir -p ${WORKSPACE}/ansible_playbooks'
            }
        }
        stage('Copy Playbooks and Inventory to Workspace Directory') {
            steps {
                // Copy the playbooks and inventory file to the created directory within the workspace
                sh 'cp prometheus.yaml inventory.yaml ${WORKSPACE}/ansible_playbooks/'
            }
        }
        stage('Execute Playbook on Ansible Server') {
            steps {
                sshagent([SSH_CREDENTIALS_ID]) {
                    // SSH into the Ansible server and execute the playbook using the copied files
                    sh """
                        ssh -o StrictHostKeyChecking=no ${ANSIBLE_USER}@${ANSIBLE_SERVER_IP} \
                        'ansible-playbook -i ${WORKSPACE}/ansible_playbooks/inventory.yaml ${WORKSPACE}/ansible_playbooks/prometheus.yaml'
                    """
                }
            }
        }
    }
}

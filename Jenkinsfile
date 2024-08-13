pipeline {
    agent any
    environment {
        ANSIBLE_SERVER_DNS = 'ec2-13-235-99-115.ap-south-1.compute.amazonaws.com'
        ANSIBLE_USER = 'jenkins'
        SSH_CREDENTIALS_ID = 'jenkins'
        AWS_CREDENTIALS_ID = 'aws'
    }
    stages {
        stage('Deploy Prod Application') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: AWS_CREDENTIALS_ID]]) {
                    script {
                        // Update kubeconfig to point to the correct EKS cluster
                        sh '''
                        aws eks update-kubeconfig --name eks-my-cluster-prod --region ap-south-1
                        '''
                        // Optional: Add sleep to ensure Kubernetes deployment is ready (consider using a wait strategy instead)
                        sleep 10
                    }
                }
            }
        }
        stage('Clone Repo and Run Playbook on Ansible Server') {
            steps {
                sshagent([SSH_CREDENTIALS_ID]) {
                    // SSH into the Ansible server, clean the directory if it exists, clone the repository, and run the playbook
                    sh """
                        ssh -o StrictHostKeyChecking=no ${ANSIBLE_USER}@${ANSIBLE_SERVER_DNS} '
                        if [ -d /home/${ANSIBLE_USER}/ansible_playbooks ]; then
                            rm -rf /home/${ANSIBLE_USER}/ansible_playbooks
                        fi &&
                        git clone https://github.com/Sharuqmd/Ansible-preparation.git /home/${ANSIBLE_USER}/ansible_playbooks &&
                        cd /home/${ANSIBLE_USER}/ansible_playbooks &&
                        ansible-playbook -i inventory.yaml prometheus.yaml
                        '
                    """
                }
            }
        }
    }
}

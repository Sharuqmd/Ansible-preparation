pipeline {
    agent any
    environment {
        ANSIBLE_SERVER_DNS = 'ec2-13-235-99-115.ap-south-1.compute.amazonaws.com' // DNS of your Ansible server
        ANSIBLE_USER = 'jenkins'             // User on the Ansible server
        SSH_CREDENTIALS_ID = 'jenkins'       // Jenkins credentials ID for SSH access
        AWS_CREDENTIALS_ID = 'aws-credentials' // Jenkins credentials ID for AWS access (replace with your actual ID)
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout code from the GitHub repository
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
        stage('Deploy Prod Application') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: AWS_CREDENTIALS_ID]]) {
                    script {
                        // Update kubeconfig to point to the correct EKS cluster
                        sh '''
                        aws eks update-kubeconfig --name eks-my-cluster-prod --region ap-south-1
                        '''
                        // Optional: Add sleep to ensure Kubernetes deployment is ready (consider using a wait strategy instead)
                        sleep 60
                    }
                }
            }
        }
        stage('Execute Playbook on Ansible Server') {
            steps {
                sshagent([SSH_CREDENTIALS_ID]) {
                    // SSH into the Ansible server using DNS name and execute the playbook using the copied files
                    sh """
                        ssh -o StrictHostKeyChecking=no ${ANSIBLE_USER}@${ANSIBLE_SERVER_DNS} \
                        'ansible-playbook -i /path/to/inventory /path/to/playbook.yaml'
                    """
                }
            }
        }
    }
}

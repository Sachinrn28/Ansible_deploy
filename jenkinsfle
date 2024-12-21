pipeline {
    agent {label 'slave-2'}
    environment {
        ANSIBLE_HOST = '172.31.25.202'        // The machine where Ansible is installed
        KUBECONFIG_PATH = '/home/ubuntu/.kube/config'    // Path to the Kubernetes kubeconfig file
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/nitinn94/ansible.git'  // Fetch your codebase
            }
        }
        stage('Run Ansible Playbook') {
            steps {
                sshagent(['Jenkins-private-key']) {
                    sh '''
                    ansible-playbook -i ${ANSIBLE_HOST}, \
                                     -e kubeconfig_path=${KUBECONFIG_PATH} \
                                     ansible/deploy.yml  # Run the Ansible playbook
                    '''
                }
            }
        }
    }
    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}

pipeline {
    agent {label 'slave1'}
    environment {
        ANSIBLE_HOST = '172.31.24.71'        // The machine where Ansible is installed
        KUBECONFIG_PATH = '/home/ubuntu/.kube/config'    // Path to the Kubernetes kubeconfig file
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Sachinrn28/Ansible_deploy.git'  // Fetch your codebase
            }
        }
        stage('Run Ansible Playbook') {
            steps {
                sshagent(['jenkins_master']) {
                    sh '''
                    ansible-playbook -i ${ANSIBLE_HOST}, \
                                     -e kubeconfig_path=${KUBECONFIG_PATH} \
                                     deploy.yml  # Run the Ansible playbook
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

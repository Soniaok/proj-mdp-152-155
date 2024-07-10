pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/Soniaok/proj-mdp-152-155'
            }
        }
        stage('Setup Build Environment') {
            steps {
                script {
                    // Print environment variables for debugging
                    sh 'printenv'
                    // Check if git is installed and its version
                    sh 'git --version'
                    // Ensure ansible is available
                    sh 'ansible --version'
                }
                ansiblePlaybook playbook: 'Build.yml', inventory: 'hosts.ini'
            }
        }
        stage('Build') {
            steps {
                sshagent(['jenkins']) {
                    script {
                        // Debugging: Print SSH agent variables
                        sh 'echo $SSH_AUTH_SOCK'
                        sh 'echo $SSH_AGENT_PID'
                    }
                    // Attempt to add the SSH key
                    sh 'ssh-add'
                }
            }
        }
        stage('Setup Deploy Environment') {
            steps {
                // Your deployment setup steps here
            }
        }
        stage('Deploy') {
            steps {
                // Your deploy steps here
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}

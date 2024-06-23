pipeline {
    agent none
    environment {
        ANSIBLE_INVENTORY = '/home/centos/hosts.ini'
    }
    stages {
        stage('Clone Repository') {
            agent { label 'Ansible-master' } // Replace with the label of your Ansible Master node
            steps {
                git branch: 'project-1', url: 'https://github.com/Soniaok/proj-mdp-152-155.git'
            }
        }
        stage('Setup Build Environment') {
            agent { label 'Ansible-master' } // Replace with the label of your Ansible Master node
            steps {
                ansiblePlaybook(
                    inventory: "${env.ANSIBLE_INVENTORY}",
                    playbook: 'Build.yml'
                )
            }
        }
        stage('Build') {
            agent { label 'Ansible-master' } // Replace with the label of your Ansible Master node
            steps {
                sshagent(['centos']) {
                    sh 'ssh centos@3.83.40.241 git "cd proj-mdp-152-155 && git checkout project-1 && mvn clean install"'
                    sh 'scp target/WebAppCal.war centos@44.202.164.83:/usr/local/tomcat9/webapps/'
                }
            }
        }
        stage('Setup Deploy Environment') {
            agent { label 'Ansible-master' } // Replace with the label of your Ansible Master node
            steps {
                ansiblePlaybook(
                    inventory: "${env.ANSIBLE_INVENTORY}",
                    playbook: 'Deploy.yml'
                )
            }
        }
        stage('Deploy') {
            agent { label 'Ansible-master' } // Replace with the label of your Ansible Master node
            steps {
                sshagent(['your-ssh-credentials-id']) {
                    sh 'ssh centos@44.202.164.83 "sudo /usr/local/tomcat9/bin/shutdown.sh && sudo /usr/local/tomcat9/bin/startup.sh"'
                }
            }
        }
    }
}

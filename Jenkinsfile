pipeline {
    agent any

    environment {
        ANSIBLE_PLAYBOOK = "wordpress.yml"  // Nom du playbook Ansible
        INVENTORY = "hosts"                    // Fichier d'inventaire Ansible
        NODES = "server"                     // Groupe d'hôtes ou variables Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Vérifier le dépôt contenant le playbook Ansible
                    checkout scm
                }
            }
        }

        stage('Install Ansible') {
            steps {
                script {
                    // Installer Ansible s'il n'est pas déjà installé
                    sh 'apt update && sudo apt install -y ansible'
                }
            }
        }
        
        stage('deploye') {
            steps {
                script {
                    // Installer Ansible s'il n'est pas déjà installé
                    sh 'ansible-playbook wordpress.yaml -e ask-pass NODES=server'
                }
            }
        }

        stage('Run Playbook') {
            steps {
                script {
                    // Exécuter le playbook Ansible pour déployer la stack WordPress et MariaDB
                    sh """
                    ansible-playbook ${ANSIBLE_PLAYBOOK} \
                    -i ${hosts} \
                    -e NODES=${NODES} \
                    """
                }
            }
        }
    }

    post {
        always {
            // Archive les logs ou résultats de déploiement
            archiveArtifacts artifacts: '**/playbook.log', allowEmptyArchive: true
        }

        success {
            echo 'Playbook executed successfully.'
        }

        failure {
            echo 'Playbook failed.'
        }
    }
}

pipeline {
    agent any

    environment {
        ANSIBLE_PLAYBOOK = "wordpress.yml"  // Nom du playbook Ansible
        INVENTORY = "hosts.ini"                    // Fichier d'inventaire Ansible
        NODES = "docker_hosts"                     // Groupe d'hôtes ou variables Jenkins
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

        stage('Run Playbook') {
            steps {
                script {
                    // Exécuter le playbook Ansible pour déployer la stack WordPress et MariaDB
                    sh """
                    ansible-playbook ${ANSIBLE_PLAYBOOK} \
                    -i ${INVENTORY} \
                    -e NODES=${NODES} \
                    -e db_volume=mariadb \
                    -e wordpress=wordpress \
                    -e mysql_root_password=secretrootpassword \
                    -e mysql_username=wordpressuser \
                    -e mysql_password=secretpassword \
                    -e mysql_database=wordpressdb \
                    -e container_port=8082
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

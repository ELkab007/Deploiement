pipeline {
    agent any

    environment {
        DB_VOLUME = 'mariadb'
        WORDPRESS_VOLUME = 'wordpress'
        MYSQL_ROOT_PASSWORD = 'secretrootpassword'
        MYSQL_USER = 'wordpressuser'
        MYSQL_PASSWORD = 'secretpassword'
        MYSQL_DATABASE = 'wordpressdb'
        CONTAINER_PORT = 8082
        NODES = 'my-server' // Modifier selon le nom des hôtes cibles
    }

    stages {
        stage('Checkout') {
            steps {
                // Récupérer le code source du dépôt Git
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    // Étape de compilation, si nécessaire
                    sh './gradlew build' // Adapter selon ton projet
                }
            }
        }

        stage('Unit Test') {
            steps {
                script {
                    // Exécuter les tests unitaires
                    sh './gradlew test' // Ou autres tests unitaires
                }
            }
        }

        stage('Integration Test') {
            steps {
                script {
                    // Exécuter les tests d'intégration
                    sh './gradlew integrationTest' // Ou autre command pour les tests d'intégration
                }
            }
        }

        stage('Create Database') {
            steps {
                script {
                    // Optionnel, si la base de données n'est pas déjà déployée par Ansible
                    echo 'Base de données sera créée par Ansible lors du déploiement.'
                }
            }
        }

        stage('Deploy with Ansible') {
            steps {
                script {
                    // Lancer Ansible pour déployer WordPress et MariaDB
                    sh '''
                    ansible-playbook -i "${NODES}," -e db_volume=${DB_VOLUME} \
                    -e wordpress=${WORDPRESS_VOLUME} -e mysql_root_password=${MYSQL_ROOT_PASSWORD} \
                    -e mysql_username=${MYSQL_USER} -e mysql_password=${MYSQL_PASSWORD} \
                    -e mysql_database=${MYSQL_DATABASE} -e container_port=${CONTAINER_PORT} \
                    deploy_wordpress.yml
                    '''
                }
            }
        }
    }

    post {
        always {
            // Nettoyage à la fin du pipeline
            script {
                echo 'Pipeline terminé, nettoyage si nécessaire.'
            }
        }

        success {
            echo 'Pipeline terminé avec succès !'
        }

        failure {
            echo 'Pipeline échoué.'
        }
    }
}

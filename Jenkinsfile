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
        NODES = '192.168.122.128' // Modifier selon le nom des hôtes cibles
    }

    stages {
        stage('Checkout') {
            steps {
                // Récupérer le code source du dépôt Git
                git url: 'https://github.com/ELkab007/Deploiement.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                script {
                    // Vérifie si le fichier gradlew est présent avant d'exécuter la commande
                    if (fileExists('gradlew')) {
                        sh './gradlew build'
                    } else {
                        echo 'Gradlew non trouvé. Passer à l’étape suivante.'
                    }
                }
            }
        }

        stage('Unit Test') {
            steps {
                script {
                    // Exécuter les tests unitaires si Gradle est utilisé
                    if (fileExists('gradlew')) {
                        sh './gradlew test'
                    } else {
                        echo 'Gradlew non trouvé. Aucun test unitaire exécuté.'
                    }
                }
            }
        }

        stage('Integration Test') {
            steps {
                script {
                    // Exécuter les tests d'intégration si Gradle est utilisé
                    if (fileExists('gradlew')) {
                        sh './gradlew integrationTest'
                    } else {
                        echo 'Gradlew non trouvé. Aucun test d’intégration exécuté.'
                    }
                }
            }
        }

        stage('Create Database') {
            steps {
                script {
                    echo 'La base de données sera créée par Ansible lors du déploiement.'
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
                    wordpress.yml
                    '''
                }
            }
        }
    }

    post {
        always {
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

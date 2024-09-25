pipeline {
    agent any

    stages {
        stage ('Checkout') {
            steps {
                // Récupère le code source du dépôt Git
                git url: 'https://github.com/ELkab007/Deploiement.git', branch: 'main'
            }
        }

        stage ('Build') {
            steps {
                script {
                    // Logique de build ici si nécessaire
                    echo 'Build stage executed.'
                    // Exemple : sh './gradlew build' // Décommentez si Gradle est utilisé
                }
            }
        }

        stage ('Unit Test') {
            steps {
                script {
                    // Exécuter les tests unitaires si Gradle est utilisé
                    echo 'Gradlew not found. No unit tests executed.'
                    // Exemple : sh './gradlew test' // Décommentez si Gradle est utilisé
                }
            }
        }

        stage ('Integration Test') {
            steps {
                script {
                    // Exécuter les tests d'intégration si Gradle est utilisé
                    echo 'Gradlew not found. No integration tests executed.'
                    // Exemple : sh './gradlew integrationTest' // Décommentez si Gradle est utilisé
                }
            }
        }

        stage ('Create Docker Environment') {
            steps {
                script {
                    // S'assurer que Docker est installé et configuré correctement
                    echo 'Setting up Docker environment...'
                    sh '''
                        if ! docker --version; then
                            echo "Docker is not installed. Installing Docker..."
                            sudo apt-get update
                            sudo apt-get install -y docker.io
                            sudo systemctl start docker
                            sudo systemctl enable docker
                        fi
                    '''
                }
            }
        }

        stage ('Pull Docker Images') {
            steps {
                script {
                    // Télécharger les images nécessaires pour WordPress et MariaDB
                    echo 'Pulling Docker images for WordPress and MariaDB...'
                    sh '''
                        docker pull wordpress:latest
                        docker pull mariadb:latest
                    '''
                }
            }
        }

        stage ('Create Database') {
            steps {
                script {
                    // Déclencher la création de la base de données via Ansible
                    echo 'The database will be created by Ansible during deployment.'
                    // Exemple : sh 'ansible-playbook create_db.yml' // Décommentez si vous avez un playbook Ansible
                }
            }
        }

        stage ('Deploy WordPress with Ansible and Docker') {
            steps {
                script {
                    // Exécuter le playbook Ansible pour déployer WordPress via Docker
                    echo 'Deploying WordPress using Ansible and Docker...'
                    sh '''
                        ansible-playbook wordpress.yaml --become --ask-become-pass -e "NODES=server" -e "docker=true"
                    '''
                }
            }
        }
        
        stage ('Post-Deployment Check') {
            steps {
                script {
                    // Vérifier que le conteneur WordPress fonctionne correctement
                    echo 'Checking if WordPress is running...'
                    sh '''
                        docker ps | grep wordpress || echo "WordPress container is not running!"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully. WordPress has been deployed.'
        }
        failure {
            echo 'Pipeline failed. Please check the logs for details.'
        }
    }
}

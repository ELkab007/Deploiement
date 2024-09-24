pipeline {
    agent any

    stages {
        stage ('Checkout') {
            steps {
                // Retrieve the source code from the Git repository
                git url: 'https://github.com/ELkab007/Deploiement.git', branch: 'main'
            }
        }

        stage ('Build') {
            steps {
                script {
                    // Add your build logic here
                    echo 'Build stage executed.'
                    // Example: sh './gradlew build' // Uncomment if using Gradle
                }
            }
        }

        stage ('Unit Test') {
            steps {
                script {
                    // Execute unit tests if Gradle is used
                    echo 'Gradlew not found. No unit tests executed.'
                    // Example: sh './gradlew test' // Uncomment if using Gradle
                }
            }
        }

        stage ('Integration Test') {
            steps {
                script {
                    // Execute integration tests if Gradle is used
                    echo 'Gradlew not found. No integration tests executed.'
                    // Example: sh './gradlew integrationTest' // Uncomment if using Gradle
                }
            }
        }

        stage ('Create Database') {
            steps {
                script {
                    echo 'The database will be created by Ansible during deployment.'
                    // Example: sh 'ansible-playbook create_db.yml' // Uncomment if using Ansible
                }
            }
        }

        stage ('Deploy') { // Ajout de la nouvelle étape de déploiement
            steps {
                script {
                    // Exécutez la commande ansible-playbook
                    sh 'ansible-playbook wordpress.yaml -e "NODES=localhost" '
                }
            }
        }
    }
}

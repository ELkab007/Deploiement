pipeline {
    agent any

    stages {
        stage ('Checkout') {
            steps {
                // Retrieve the source code from the Git repository
                git url: 'https://github.com/ELkab007/Deploiement.git', branch: 'main'
            }
        }

        stage ('Run Ansible') {
            steps {
                script {
                    // Add your build logic here
    credentialsId:
    '065dd48d-93d4-431d-9ce5-d3c53917adf7', inventory:
    'hosts', playbook:
    'wordpress.yaml'
                    // Example: sh './gradlew build' // Uncomment if using Gradle

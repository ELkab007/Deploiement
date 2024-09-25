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
    'ssh-key' , inventory:
    'hosts', playbook:
    'wordpress.yaml'
                    // Example: sh './gradlew build' // Uncomment if using Gradle

pipeline {
    agent any

   

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Example build step
                echo 'Building the project...'
                'make build'  // Replace with your build command
            }
        }

        stage('Test') {
            steps {
                // Example test step
                echo 'Running tests...'
                 'make test'  // Replace with your test command
            }
        }

        stage('Deploy') {
            steps {
                // Example deployment step
                echo 'Deploying the application...'
                'make deploy'  // Replace with your deploy command
   

pipeline {
    agent any
    stages {
        stage('checkout') {
            steps{
                 echo "this is checkout"
            }     
        }
        stage('Build') {
            steps {
                echo "This is building step"
            }
        }
        stage('Test') {
            steps {
                echo "this is testing step"
            }
        }
        stage('Deploy') {
            steps {
                echo "this is building step.."
            }
        }
    }
    post {
        always {
            echo "this is just a step"
        }
        success {
            emailext(
                subject: "Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}",
                body: "Build Status: ${currentBuild.currentResult}\nCheck the console output at ${env.BUILD_URL}",
                to: "oladapper92gmail.com",
                replyTo: "oladapper92@gmail.com",
                from: "oladapper@gmail.com"
            )
        }
        failure {
            emailext(
                subject: "Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}",
                body: "Build Status: ${currentBuild.currentResult}\nCheck the console output at ${env.BUILD_URL}",
                to: "oladapper92@gmail.com",
                replyTo: "oladapper@92@gmail.com",
                from: "oladapper@gmail.com"
            )
        }
    }
}
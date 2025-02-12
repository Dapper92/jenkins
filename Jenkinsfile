pipeline {
    agent any
    environment
      USER_CUSTOM_VARIABLE="james"
      ANOTHER_CUSTOM_VARIABLE="john"
    stages {
        stage('Build') {
            steps {
                echo "This is building stpe"
            }
        }
        stage('Testing') {
            when {
                expression{
                       BRANCH_NAME == "testing"
                } "this is testing step"
            }
        }
        stage('Deploy') {
            steps {
                echo "this is building step"
            }
        }
    }
}
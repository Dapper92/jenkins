pipeline {
    agent any
    stages {
        stage('starting') {
            steps {
                echo "This is starting step"
            }
        }
        stage('building') {
            when {
                expression{
                       BRANCH_NAME == "testing"
                }
            }
            steps{ 
                echo "this is for the building stage for testing branch"
            }
        }
        stage('production') {
            steps {
                echo "this is production step"
            }
        }
    }
}
pipeline {
    agent any
    stages {
        stage('checkout') {
            steps {
                echo "This is checkout"
            }
        }
        stage('Build') {
            steps {
                echo "This is building step"
            }
        }
        stage('Test') {
            steps {
                echo "This is testing step"
            }
        }
        stage('Deploy') {
            steps {
                script {
                    try {
                        sh "ssh -o -i tester.key root@123.222.222.23.22"
                    } catch (err) {
                        echo "Deployment failed"
                        throw err
                    }
                }
            }
        }
    }
    
    post {
        success {
            emailext(
                subject: "Build ${currentBuild.currentResult}: Job ${env.JOB_NAME} #${BUILD_NUMBER}",
                body: "Build Status: ${currentBuild.currentResult}\nCheck the console output at ${env.BUILD_URL}",
                to: "oladapper92@gmail.com",
                replyTo: "oladapper92@gmail.com",
                from: "oladapper@gmail.com"
            )
        }
        failure {
            script {
                def build_log = currentBuild.rawBuild.getLog(100).join('\n')
                emailext(
                    subject: "Build Failed: ${env.JOB_NAME} #${BUILD_NUMBER}",
                    body: """Build Status: ${currentBuild.currentResult}
                    Check the console output at: ${env.BUILD_URL}
                    Last 100 lines of build log:
                    ${build_log}
                    """,
                    to: "oladapper92@gmail.com",
                    replyTo: "oladapper92@gmail.com",
                    from: "oladapper@gmail.com"
                )
            }
        }
    }
}

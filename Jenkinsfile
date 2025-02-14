pipeline {
    agent any
    stages {

        tools {
            nodejs "node18"
        }
        stage('checkout'){
            git "https://github.com/Dapper92/jenkins.git"
        }
        stage("starting"){
            steps {
                echo "This is the starting stage"
            }
        }
        stage('Build') {
            when{
                expression{
                    BRANCH_NAME == "testing"
                }
            }
            
         
            steps {
                script {
                    try {
                        sh "npm run test | tee build.log"
                    }catch (err) {
                        currentBuild.result = "Failure"
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
                // def build_log = currentBuild.rawBuild.getLog(100).join('\n')
                def build_log =readFile("build.log")
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

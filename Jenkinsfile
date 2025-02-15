pipeline {
    agent any

    tools {
        nodejs "node 18"
    }

    stages {
        stage("checkout") {
            steps {
                git "https://github.com/Dapper92/jenkins.git"
            }
        }

        stage("starting") {
            steps {
                echo "This is the starting stage"
            }
        }

        stage("Build") {
            when {
                expression {
                    env.BRANCH_NAME == "testing"  // ✅ Corrected BRANCH_NAME reference
                }
            }
            steps {
                script {
                    try {
                        sh "npm run test | tee build.log"
                    } catch (err) {
                        echo "error is ${err.getMessage}"
                        throw err
                    }
                }
            }
        }
    }

    // post {
    //     success {
    //         emailext(
    //             subject: "Build ${currentBuild.currentResult}: Job ${env.JOB_NAME} #${BUILD_NUMBER}",
    //             body: "Build Status: ${currentBuild.currentResult}\nCheck the console output at ${env.BUILD_URL}",
    //             to: "oladapper92@gmail.com",
    //             replyTo: "oladapper92@gmail.com",
    //             from: "oladapper@gmail.com"
            
        }
        failure {
            script {
                def build_log = readFile("build.log")  // ✅ Reads log file for failure email
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

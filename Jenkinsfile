pipeline {
    agent any
        tools {
            nodejs "node18"
    }

    stages {
        stage('checkout') {
            steps{
                 git "https://github.com/Oluwaseun186/jenkinsfile.git"
            }     
        }

        stage('building') {
             when{
                 expression{
                    BRANCH_NAME == "testing."
                 }
             }
            steps {
                script{
                    try{
                        sh 'touch build.log'
                        sh "npm install"
                        sh "npm run build"
                        sh "npm run test"
                       
                    }catch(Exception err){
                        echo "error is ${err.getMessage}"
                        throw err
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "this is building step.."
            }
        }
    }

    post {
        // success {
        //     emailext(
        //         subject: "Build ${currentBuild.currentResult}: Job ${env.JOB_NAME} #${BUILD_NUMBER}",
        //         body: "Build Status: ${currentBuild.currentResult}\nCheck the console output at ${env.BUILD_URL}",
        //         to: "oladapper92@gmail.com",
        //         replyTo: "oladapper92@gmail.com",
        //         from: "oladapper@gmail.com"
        //     )   
        // }
        failure {
                script{
                     //def build_log = currentBuild.rawBuild.getLog(100).join('\n') 
                     //def build_log = Manager.build.log
                     def build_log = readFile("build.log")
                        emailext(
                            subject: "Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}, ${env.BUILD_NUMBER}, ${JOB_NAME}, ${build_log},  ${BUILD_URL}",
                            body: "Build Status: ${currentBuild.currentResult}\nCheck the console output at ${env.BUILD_URL}",
                            to: "oladapper92@gmail.com",
                            replyTo: "oladapper@gmail.com",
                            from: "oladapper@gmail.com"
                        )                    
                }
        }
    }
}

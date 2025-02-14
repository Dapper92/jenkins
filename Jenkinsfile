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
                script {
                    try {
                        sh "ssh -o -i tester.key root@123.222.222.23.22"
                       }catch(err)
                       if(err){ 
                           currentBuild.Result="failure"
                        throw(err)   

                    }
                } 
                
            }
        }
    }
    post {
        
        }
        success {
            emailext(
                subject: "Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}, ${BUILD_NUMBER},${env.BUILD_LOG}",
                body: "Build Status: ${currentBuild.currentResult}\nCheck the console output at ${env.BUILD_URL}",
                to: "oladapper92gmail.com",
                replyTo: "oladapper92@gmail.com",
                from: "oladapper@gmail.com"
            )
        }
        failure {
            script {
                def build log = currentBuild.rawBuild.getLog(200).join('\n')
            }
            emailext(
                subject: "everything failed"
                body: "this is the default body. ${env.JOB_NAME} - ${env.BUILD_NUMBER},
                       ${env.BUILD_URL}

                       ${env.BUILD_LOG}
                to: "oladapper92@gmail.com",
                replyTo: "oladapper92@gmail.com",
                from: "oladapper@gmail.com"
                
            )
        }
    }
}
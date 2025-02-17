pipeline {
    agent any

    tools {
        nodejs "node 18"
    }

    environment {
        IMAGE_NAME = "dapper01/new-test-image"
        IMAGE_TAG = "1"
    }

    stages {
        stage("checkout") {
            steps {
                git branch: "testing", url: "https://github.com/Dapper92/jenkins.git"
            }
        }

        stage("starting") {
            steps {
                echo "This is the actual starting stage"
            }
        }

        stage("Build") {
            when {
                expression {
                    env.BRANCH_NAME == "testing"
                }
            }
            // steps {
            //     // sh "npm init -y"
            //     // script {
            //     //     try {
            //     //         sh "npm run start"
            //     //         sh "npm run test | tee builder.log"
            //     //     } catch (Exception err) {
            //     //         sh "echo ${err} | tee builder.log"
            //     //         throw err
            //     //     }
            //     // }
            // }
        }

        stage("Docker Build") {
            when {
                expression {
                    env.BRANCH_NAME == "testing"
                }
            }
            steps {
                script {
                    echo "Building Docker image: ${IMAGE_NAME}:${IMAGE_TAG}"
                    
                    // Build Docker image
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."

                    // Use Jenkins credentials to log in to DockerHub securely
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh "echo 'Logging into DockerHub securely...'"
                        sh "docker login -u $DOCKER_USER -p $DOCKER_PASS"
                    }

                    // Push Docker image
                    sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }

    post {
        success {
            script { 
                def build_log = readFile("builder.log")
                emailext(
                    subject: "Build ${currentBuild.currentResult}: Job ${env.JOB_NAME} #${BUILD_NUMBER}",
                    body: "Build Status: ${currentBuild.currentResult}\nCheck the console output at ${env.BUILD_URL}",
                    to: "oladapper92@gmail.com",
                    replyTo: "oladapper92@gmail.com",
                    from: "oladapper@gmail.com"
                )   
            }
        }
        
        failure {
            script {
                def build_log = readFile("builder.log")
                emailext(
                    subject: "Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}, ${env.BUILD_NUMBER}",
                    body: """Build Status: ${currentBuild.currentResult}
                    Check the console output at: ${env.BUILD_URL}
                    Last 100 lines of build log:
                    ${build_log}
                    """,
                    to: "oladapper92@gmail.com",
                    replyTo: "oladapper@gmail.com",
                    from: "oladapper@gmail.com"
                )                    
            }
        }
    }
}
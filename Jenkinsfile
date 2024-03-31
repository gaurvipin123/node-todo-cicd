pipeline {
    agent any
    
    stages {
        
        stage("code"){
            steps{
                git url: "https://github.com/gaurvipin123/node-todo-cicd.git", branch: "master"
                echo 'bhaiyya code clone ho gaya'
            }
        }
        stage("build and test"){
            steps{
                sh "docker build -t node-app-test-new ."
                echo 'code build bhi ho gaya'
            }
        }
        stage("scan image"){
            steps{
                echo 'image scanning ho gayi'
            }
        }
        stage("push"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker tag node-app-test-new:latest ${env.dockerHubUser}/node-app-test-new:latest"
                sh "docker push ${env.dockerHubUser}/node-app-test-new:latest"
                echo 'image push ho gaya'
                }
            }
        }
        stage("deploy"){
            steps{
                sh "docker run -d -p 8000:8000 node-app-test-new:latest"
                echo 'deployment ho gayi'
            }
        }
    }

     post {
        always {
            script {
                def emailSubject
                def emailBody
                def recipientEmail

                if (currentBuild.result == "SUCCESS") {
                    emailSubject = "Pipeline Success: ${env.JOB_NAME} - Build #${env.BUILD_NUMBER}"
                    emailBody = "The pipeline run for ${env.JOB_NAME} - Build #${env.BUILD_NUMBER} was successful. You can view the details at ${env.BUILD_URL}"
                    recipientEmail = "ananda.yashaswi@quokkalabs.com, vipinkumargaurmca09@gmail.com"
                }
                else {
                    emailSubject = "Pipeline Failure: ${env.JOB_NAME} - Build #${env.BUILD_NUMBER}"
                    emailBody = "The pipeline run for ${env.JOB_NAME} - Build #${env.BUILD_NUMBER} has failed. You can view the details at ${env.BUILD_URL}"
                    recipientEmail = "ananda.yashaswi@quokkalabs.com, vipinkumargaurmca09@gmail.com"
                }

                emailext (
                    subject: emailSubject,
                    body: emailBody,
                    to: recipientEmail
                )
            }
        }
    }
}

pipeline {
    agent any
    
    stages {
        
        stage("code"){
            steps{
                git url: "https://github.com/LondheShubham153/node-todo-cicd.git", branch: "master"
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
        failure {
              emailext body: '''${SCRIPT, template="groovy-html.template"}''', 
                       subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Failed", 
                       mimeType: 'text/html',to: "gaurvipin321@gmail.com"
       }
       success {
             emailext body: '''${SCRIPT, template="groovy-html.template"}''', 
                      subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Successful", 
                      mimeType: 'text/html',to: "gaurvipin321@gmail.com"
       }      
    }
}

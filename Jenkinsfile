pipeline {
    agent any
    environment {
        mavenhome = tool name: 'localmaven', type: 'maven'
    }
    stages{
        stage('BuildTest'){
            steps{
                sh "${mavenhome}/bin/mvn clean package"
                sh "docker build -t tomcatwebapp:${env.BUILD_ID} ."
            }
        }
        stage('Deploy'){
            steps{
                sh "if docker ps | grep webapp; then docker stop webapp; docker rm webapp; else echo 'creating container'; fi"
                sh "docker run -itd --name webapp -p 8082:8080 tomcatwebapp:${env.BUILD_ID}"
            }
        }
    }
}

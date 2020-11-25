node
{
    properties([[$class: 'JiraProjectProperty'], [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
def mavenHome = tool name:"maven3.6.3"    
    stage('CheckoutCode'){
        git branch: 'development', credentialsId: '3b5d87a2-1406-4f1e-ba8c-4b42610259be', url: 'https://github.com/Prasanth-21/maven-web-application.git'
    }
    stage('Build'){
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage('Execute sonarQubeReport'){
        sh "${mavenHome}/bin/mvn sonar:sonar"   
    }
    stage('uploadArtifact Into Nexus'){
        sh "${mavenHome}/bin/mvn deploy"
    }
    stage('DeployApp to Tomcat'){
        sshagent(['72707f58-d188-49a1-b2f6-1751082229ec'])
        {
            sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@35.154.215.199:/opt/apache-tomcat-9.0.40/webapps/"
        }
    }
}


node{
   properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), 

   pipelineTriggers([pollSCM('* * * * *')])])
    
    def mavenHome = tool name : "maven3.8.4"
    
    stage('checkoutcode'){
        git branch: 'development', credentialsId: '3a9010e9-672a-4046-8ec2-eac91ae57580', url: 'https://github.com/kv-devops-dec2021/maven-web-application.git'
    }
    stage('buildcheckoutcode'){
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage('executesonarqubereport'){
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage('updateartifactorynexusrepo'){
        sh "${mavenHome}/bin/mvn deploy"
    }
    stage('deploymentintomcat'){
        sshagent(['3ea09190-1d53-40f9-8fa6-e327fb46fcdc']) {
       sh "scp -o StrictHostKeyChecking=no  target/maven-web-application.war ec2-user@ip-172-31-6-241:/opt/apache-tomcat-9.0.58/webapps"
}
    }
}

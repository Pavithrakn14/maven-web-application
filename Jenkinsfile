node {
echo "Job name is: ${env.JOB_NAME}"
echo "Build Number is: ${env.BUILD_NUMBER}"
echo "Node Name is: ${env.NODE_NAME}"
//echo "Jenkins Home dir is: ${env.JENKINS_HOME}"
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

def mavenHome= tool name: "maven3.9.0"

stage ('checkoutcode') {
git branch: 'development', credentialsId: '872dc8ee-8880-49bd-b988-8ff62f5d45f2', url: 'https://github.com/Pavithrakn14/maven-web-application.git'
}

stage ('Build') {
sh "${mavenHome}/bin/mvn clean package"
}
/*
stage ('SonarqubeReport') {
sh "${mavenHome}/bin/mvn clean sonar:sonar package"
}

stage ('uploadArtifactoryintoNexus') {
sh "${mavenHome}/bin/mvn clean deploy" 
}

stage ('DeploytheAppIntoTomcatServer') {
sshagent(['519500fc-8260-42ab-b577-90e89b4e9d14']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.9.214:/opt/apache-tomcat-9.0.76/webapps/"
}
}
*/
}


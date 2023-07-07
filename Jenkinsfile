node {

echo "Job name is: ${env.JOB_NAME}"
echo "Build Number is: ${env.BUILD_NUMBER}"
echo "Node Name is: ${env.NODE_NAME}"
echo "Jenkins Home dir is: ${env.JENKINS_HOME}"

def mavenHome= tool name: "maven3.9.0"

try
{
stage ('checkoutcode') {
sendSlackNotifications('STARTED')
git branch: 'development', credentialsId: '872dc8ee-8880-49bd-b988-8ff62f5d45f2', url: 'https://github.com/Pavithrakn14/maven-web-application.git'
}

stage ('Build') {
    
sh "${mavenHome}/bin/mvn clean package"
}
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

}
catch (e)
{
currentBuild.result = "FAILED"
throw e 
}
finally{
sendSlackNotifications(currentBuild.result)
}

}// node closing 



def sendSlackNotifications(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}



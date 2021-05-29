node('nodes'){
    def mavenHome = tool name: "maven3.8.1"
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
stage('CheckoutCode'){
  git branch: 'development', credentialsId: '43c9b3ec-0c2e-4c41-a213-594380c9bc4f', url: 'https://github.com/JavaApplication-mss/maven-web-application.git'
}
stage('Build'){
 sh "${mavenHome}/bin/mvn clean package"
}
stage('ExecuteSonarQubeReport'){
  sh "${mavenHome}/bin/mvn clean package sonar:sonar"
}
stage('UploadArtifactIntoNexus'){
sh "${mavenHome}/bin/mvn clean clean package sonar:sonar deploy"
}
stage('UploadAppIntoTamcatServer'){
sshagent(['4afa9fa9-defa-43e6-bf6f-d62aab9b4666']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.235.99.236:/opt/apache-tomcat-9.0.46/webapps/"
}
}

stage('SendEmailNotification'){
emailext body: '''Build Over...

Regards,
Dhananjaya''', subject: 'BuildOver', to: 'dhananjayareddy353@gmail.com, mechdhana61@gmail.com'
}
}

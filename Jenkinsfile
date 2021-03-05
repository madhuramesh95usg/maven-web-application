node
{

def mavenHome = tool name: "maven3.6.3"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '3', daysToKeepStr: '', numToKeepStr: '3')), pipelineTriggers([pollSCM('* * * * * ')])])





stage( 'CHECKOUT THE CODE' )
{
git branch: 'development', credentialsId: '96794d51-ffbf-4a77-89c1-3ae358f2b908', url: 'https://github.com/madhuramesh95usg/maven-web-application.git'
}

stage( 'BUILD' )
{
sh "${mavenHome}/bin/mvn clean package"
}

stage( 'EXECUTE SONARQUBE REPORT' )
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage( 'UPLOAD artifacts into Nexus' )
{
sh "${mavenHome}/bin/mvn deploy"
}

stage( 'DEPLOY APP INTO TOMCAT SERVER' )
{
sshagent(['82a1bd50-05a6-4adc-859e-29cf9a65a68c']) {
  sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@52.66.210.76:/opt/apache-tomcat-9.0.41/webapps/"
}
}

stage( 'SEND EMAIL NOTIFICATION' )
{
emailext body: '''BUILD OVER!...

REGARDS,
RAMESH TECHNOLOGIES,
8688551572.''', subject: 'BUILD OVER...', to: 'ramesh95usg@gmail.com'
}

}

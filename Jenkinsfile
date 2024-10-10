node {

def mavenHome = tool name: 'maven3.9.9'
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '5', numToKeepStr: '5')), pipelineTriggers([cron('* * * * *')])])

echo "job name is ${env.JOB_NAME}"
echo "Build number is ${env.BUILD_NUMBER}"

stage('checkout'){
git branch: 'development', credentialsId: '907f455d-91a8-47dd-afa8-94a844399060', url: 'https://github.com/Sunilk1985/maven-web-application.git'
}
stage('build'){
sh "$mavenHome/bin/mvn clean package"
}
stage ('executeSonarQubeReport'){

sh "$mavenHome/bin/mvn clean sonar:sonar"
}
stage('UploadArtifactsIntoNexus'){
sh "$mavenHome/bin/mvn clean deploy"

}
stage('DeployArtifactsToTomcatServer'){
sshagent(['104174b1-fd72-456f-af30-363a0f844b26']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.11.1:/opt/apache-tomcat-9.0.95/webapps/"

}

}
}

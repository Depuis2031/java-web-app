pipeline{

environment {
    registry = "depuis2031/java-web-app"  
    dockerImage = ''
    registryCredential = 'DOCKERHUB-CRED'
  }

agent any
tools{
maven "maven3.9.1"
}

stages{

stage('1CodeClone'){
steps{
sh "echo 'Clonning code from GitHub'"
git "https://github.com/depuis2031/maven-web-app.git"
}
}

/*
stage('2Test+Build'){
steps{
sh "echo 'Creating an Artifacts'"
sh "mvn clean package"

}
}

stage('3CodeQuality'){
steps{
sh "echo 'Performing code Quality Review and Analysis'"
sh "mvn sonar:sonar"
}
}

stage('4Upload2Nexus'){
steps{
sh "echo 'Uploading Artifacts to Nexus'"
sh "mvn deploy"
}
}
*/
stage('Build dockerImage'){
steps{
script {
dockerImage = docker.build registry + ":$BUILD_NUMBER" //The BuildNumber will act as tag
}  
}
}

stage('PushImage2 DockerHub') {
steps{    
script {
docker.withRegistry( '', registryCredential ) {
dockerImage.push()
sh "echo 'Image successfully pushed to DockerHub'"

}
}
}
}

stage('Remove DockerImages'){
steps{
sh 'docker rmi -f $(docker images -q)'

}
}

}
}

/*
post{
always{emailext body: '''Hi All,

Please kindly review the build status and act accordingly.

Thanks
Obinna''', recipientProviders: [buildUser(), contributor(), culprits(), previous(), developers(), upstreamDevelopers(), requestor(), brokenTestsSuspects(), brokenBuildSuspects()], subject: 'Build Status', to: 'testteam@gmail.com'
}
success
{emailext body: '''Hi All,

T.

Thanks
Obinna''', recipientProviders: [buildUser(), contributor(), culprits(), previous(), developers(), upstreamDevelopers(), requestor(), brokenTestsSuspects(), brokenBuildSuspects()], subject: 'Build Status', to: 'testteam@gmail.com'
}


failure{
emailext body: '''Hi All,

Please kindly review the build status and act accordingly.

Thanks
Obinna''', recipientProviders: [buildUser(), contributor(), culprits(), previous(), developers(), upstreamDevelopers(), requestor(), brokenTestsSuspects(), brokenBuildSuspects()], subject: 'Build Status', to: 'testteam@gmail.com'

}


}

}
*/

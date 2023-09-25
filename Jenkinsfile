node ('slaves') {
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '5', numToKeepStr: '5')), pipelineTriggers([githubPush()])])
    def mavenhome = tool name:'maven3.9.4'
stage ('checkout code'){
   git credentialsId: 'f921670b-d7cd-49c4-8aff-05478ff7a2bf', url: 'https://github.com/validevops555/maven-web-application.git'
}

stage ('Build'){
    sh "${mavenhome}/bin/mvn clean package"
}

stage ('Execute sonarqube report'){
    sh "${mavenhome}/bin/mvn sonar:sonar"
}

stage ('Uploading artifacts into artifact repository'){
    sh "${mavenhome}/bin/mvn deploy"
}

stage ('Deploying application'){
  sshagent(['50345cfd-ae6a-4eac-aa1d-9feb37f88769']) {
  sh "scp -o StrictHostkeyChecking=no target/maven-web-application.war ubuntu@13.233.255.118:/opt/tomcat9/webapps/"
  }    
}
}

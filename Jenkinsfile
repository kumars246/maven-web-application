node{
    
   properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], 
   
   pipelineTriggers([pollSCM('* * * * *')])])
    
    def mavenHome = tool name: "Maven 3.8.6"
    
 stage('code pulling'){
     git branch: 'development', credentialsId: 'a939eaf6-abc1-4a94-8343-a222340f14ec', url: 'https://github.com/kumars246/maven-web-application.git'
}

stage('building the packages'){
    sh "${mavenHome}/bin/mvn clean package"
}

stage('pushing the code to sonarqube'){
    sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('pushing to Tomcat'){
    sshagent(['33dd08a1-7fe8-457e-980e-12b02ef0af8d']) {

sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@65.1.1.109:/opt/apache-tomcat-9.0.64/webapps"

}
}

}

node
{
def mavenHome = tool name: "maven3.6.3"
    
    stage("1. git clone")
    {
       git credentialsId: 'gitCredentials', url: 'https://github.com/LandmakTechnology/maven-web-app.git'
    }
    
    stage("2. Build")
    {
        sh "${mavenHome}/bin/mvn clean package"

        // bat "mvn clean package"  - for winows OS
    }
    stage('3. Sonar')
    {
         sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage('4. Nexus')
     {
        sh "${mavenHome}/bin/mvn deploy"
    }
    stage('5. Deploy')
    {
       deploy adapters: [tomcat9(credentialsId: 'TomcatCredentials', path: '', url: 'http://34.239.176.96:8080/')], contextPath: null, war: '**/maven-web-app.war'
       //sh "scp -o StrictHostKeyChecking=no target/maven-web-app ec2-user@34.204.166.28:/opt/apache-tomcat-9.0.37/webapps/"
        
    }
    stage('6. Email notification')
    {
emailext body: '''
Build Status
Landmark Technology
+ 1 437 215 2483\'\'\', recipientProviders: [developers()], subject: \'Build status\', to: \'legah2000@gmail.com\'''', recipientProviders: [developers()], subject: 'Build', to: 'legah2002@gmail.com'   
    }
}

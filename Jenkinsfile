node{

echo "Job Name Is: ${env.JOB_NAME}"
echo "Node name is: ${env.NODE_NAME}"

def mavenHome = tool name: 'maven3.8.4'

//Get the code from GitHub Repo
stage('CheckoutCode'){
git branch: 'development', credentialsId: '0f2517d2-c479-436c-ba5d-564461e84452', url: 'https://github.com/Mural-Technologies/maven-web-application.git'

}

//Do the build by using Maven Build tool
stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

//Excute the SonQube Report
stage('Excute the SonQube Report'){
sh "${mavenHome}/bin/mvn sonar:sonar"
}

//Upload Artifact into Artifactory Repo
stage('uploadArtifactintoNexus'){
sh "${mavenHome}/bin/mvn deploy"
}

//DeployApplicationIntoTomcatServer
stage('DeployApplicationIntoTomcatServer'){
sshagent(['6ab625c1-455c-4ecd-a688-f4af01aaabd9']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@18.170.87.114:/opt/apache-tomcat-9.0.62/webapps/"

}

}


}

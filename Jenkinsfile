node {
stage ('SCM Checkout' ) {
git credentialsId: 'git_creds2', url: 'https://github.com/johnsoncls2019/start_spring_new2.git'
}
stage ('Mvn package') {
def mvnHome = tool name: 'localMaven', type: 'maven' 
def mvnCMD = "${mvnHome}/usr/bin/mvn"
sh " mvn clean package"
}
stage ('Build Docker image') {
sh "docker build -t springbootnew1 ."
}
stage ('Push docker image') {
withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerHubPwd')]) {
sh "docker login -u johnsoncls2019 -p ${dockerHubPwd}"
}
sh 'docker push johnsoncls2019/springbootnew1'
}
stage ('Run Container on AWS Server') {
def dockerRun = 'docker run -p 7000:7000 -d -t --name AchistarTecnologies2 johnsoncls2019/springbootnew1'
def dockerRemove = 'docker rm --force AchistarTecnologies2'
sshagent(['server_aws1']) {
sh "ssh -o StrictHostKeyChecking=no ubuntu@3.83.30.155 ${dockerRemove}"
sh "ssh -o StrictHostKeyChecking=no ubuntu@3.83.30.155 ${dockerRun}"
}
}
stage ('Run container on Dev server') {
def dockerRemove = 'docker rm --force AchiStarTechnologies2'
def dockerRun = 'docker run -p 7000:7000 -d  -t --name AchiStarTechnologies2 johnsoncls2019/springbootnew1'
sshagent(['dev-server']) {
sh "ssh -o StrictHostKeyChecking=no root@192.168.44.129 ${dockerRemove}"
sh "ssh -o StrictHostKeyChecking=no root@192.168.44.129 ${dockerRun}"
}
}
}

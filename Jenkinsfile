node {
    properties([
    buildDiscarder(logRotator(daysToKeepStr: '1', numToKeepStr: '1')),
])
stage('Continuous Download') {
git branch: 'Development', url: 'https://github.com/MyCompany0803/DevOps_Project_1.git'
}
stage('Build') {
sh 'mvn clean package'
}
stage ('Build Docker Image'){
sh 'docker build -t myimage . '
}
stage ('Docker Login & Push Image to Docker Hub') {
     withCredentials([string(credentialsId: 'dockeruser', variable: 'Dockerpass')]) {
sh 'docker login -u kulkarniatish -p ${Dockerpass}'
sh 'docker tag myimage kulkarniatish/learning:latest'
sh 'docker push kulkarniatish/learning:latest '
}
}
stage ('Continuous Deployment on Test Server ') {
sshagent(['sshagent']) {
withCredentials([string(credentialsId: 'dockeruser', variable: 'dockerpwd')]) {
sh 'ssh -o StrictHostKeyChecking=no ec2-user@35.89.18.117 sudo docker login -u kulkarniatish -p ${dockerpwd}'
sh 'ssh -o StrictHostKeyChecking=no ec2-user@35.89.18.117 sudo docker rm -f container_name || true'
sh 'ssh -o StrictHostKeyChecking=no ec2-user@35.89.18.117 sudo docker rmi -f kulkarniatish/learning:latest || true'
sh 'ssh -o StrictHostKeyChecking=no ec2-user@35.89.18.117 sudo docker run -itdp 8080:8080 --name container_name kulkarniatish/learning:latest'
}
}
}
}

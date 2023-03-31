node { 
    

    stage('Continuous Download') {
        
            git branch: 'main', url: 'https://github.com/vyom-Labs-pvt-ltd/DevOps.git'
         
        }
    stage('Continuous Build') {
    
    def mavenHome= tool name: "Maven" ,type: "maven"
        sh "${mavenHome}/bin/mvn clean package"
		}


    stage('Build Docker Image') {

        sh 'docker build -t myapp-image . '
         
        }
    stage('Login & Push Image to Dockerhub ') {
        
	   withCredentials([string(credentialsId: 'DockerHub', variable: 'DockerPass')]) {
         sh 'docker login -u imruturaj -p ${DockerPass}'
		 sh 'docker tag myapp-image imruturaj/project1'
		 sh 'docker push imruturaj/project1'
            
        }
    }
   
	stage('Continuous Delivery (Test Server)') {
	
		input message: 'Waiting For Approval ', submitter: 'Manager'
		sshagent(['sshagent']) {
		withCredentials([string(credentialsId: 'DockerHub', variable: 'DockerPass')]) {
		sh 'ssh -o StrictHostKeyChecking=no ec2-user@172.31.28.254 sudo docker login -u imruturaj -p ${DockerPass}'
		sh 'ssh -o StrictHostKeyChecking=no ec2-user@172.31.28.254 sudo docker rm -f myapp-container || true'
		sh 'ssh -o StrictHostKeyChecking=no ec2-user@172.31.28.254 sudo docker rmi -f imruturaj/project1 || true'
		sh 'ssh -o StrictHostKeyChecking=no ec2-user@172.31.28.254 sudo docker run -p 8080:8080 -d --name myapp-container imruturaj/project1'
		
}
}
	
	
	}
}

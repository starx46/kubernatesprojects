def remote = [:]
remote.name = "ansible-server"
remote.host = "172.31.88.95"
remote.allowAnyHosts = true

node { 
    
    environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub')
	}
    
        stage('checkout scm'){
           git branch: 'main', url: 'https://github.com/starx46/kubernatesprojects.git'
           sh 'ls -ltrh' 
        
        }
  
    
    withCredentials([sshUserPrivateKey(credentialsId: 'docker-build', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'userName')]) {
        remote.user = userName
        remote.identityFile = identity
        stage("docker image build") { 
           
            sh 'mv Dockerfile Dockerfile_${BUILD_ID}'         
            sshPut remote: remote, from: "Dockerfile_${BUILD_ID}", into: '/root/docker/'
            sshCommand remote: remote, command: "docker build -t testweb:latest -f /root/docker/Dockerfile_${BUILD_ID} ."
	    //sshCommand remote: remote, command: "docker tag testweb learndockerwithme/testweb:latest"
	    sshCommand remote: remote, command: "docker tag testweb learndockerwithme/testweb:v1.${BUILD_ID}"
            
            
      stage('Docker Push') {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
	sshCommand remote: remote, command: "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
	sshCommand remote: remote, command: "docker push learndockerwithme/testweb:v1.${BUILD_ID}"
                sh 'rm -rf Dockerfile_${BUILD_ID}'
		
	stage('container creation'){
		
		sshCommand remote: remote, command: "ansible-playbook /root/docker/docker.yml -e image_id=learndockerwithme/testweb:v1.${BUILD_ID} -e container_name=testweb_v1_${BUILD_ID} -e version=v1.${BUILD_ID}"
		

		
		}
		
	
      }            
            }
        }
    }
}

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
           
            //sh 'mv Dockerfile Dockerfile_${BUILD_ID}'         
            //sshPut remote: remote, from: "Dockerfile_${BUILD_ID}", into: '/root/docker/'
	    sshPut remote: remote, from: "Dockerfile", into: '/root/docker/'
	    sshPut remote: remote, from: 'deployment.yml', into: '/root/docker/'
	    sshPut remote: remote, from: 'service.yml', into: '/root/docker/'
            //sshCommand remote: remote, command: "docker build -t testweb:latest -f /root/docker/Dockerfile_${BUILD_ID} ."
	    sshCommand remote: remote, command: "docker build -t ${JOB_NAME}:v1.${BUILD_ID} -f /root/docker/Dockerfile ."

	    sshCommand remote: remote, command: "docker tag ${JOB_NAME}:v1.${BUILD_ID} learndockerwithme/${JOB_NAME}:v1.${BUILD_ID}"
	    sshCommand remote: remote, command: "docker tag ${JOB_NAME}:v1.${BUILD_ID} learndockerwithme/${JOB_NAME}:latest"
            
            
      stage('Docker Push') {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
	sshCommand remote: remote, command: "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
	sshCommand remote: remote, command: "docker push learndockerwithme/${JOB_NAME}:v1.${BUILD_ID}"
	sshCommand remote: remote, command: "docker push learndockerwithme/${JOB_NAME}:latest"

                //sh 'rm -rf Dockerfile_${BUILD_ID}'
	sshCommand remote: remote, command: "rm -rf Dockerfile_${BUILD_ID}"
		
	stage('container creation'){
		
		//sshCommand remote: remote, command: "ansible-playbook /root/docker/docker.yml -e image_id=learndockerwithme/${JOB_NAME}:latest -e container_name=${JOB_NAME}"
		//sshCommand remote: remote, command: "ansible-playbook /root/docker/k8-deployment.yml"

		//sh 'rm -rf *'

		
		}
		
	
      }            
            }
        }
    }
}

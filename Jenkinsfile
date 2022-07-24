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
            sh 'echo ${BUILD_ID}'
            sh 'echo docker user: ${DOCKERHUB_CREDENTIALS_USR}'
            sh 'echo docker pass: $DOCKERHUB_CREDENTIALS_PSW'
            sh 'mv Dockerfile Dockerfile_${BUILD_ID}'
            //writeFile file: 'abc.sh', text: '${BUILD_NUMBER}'
            //sshScript remote: remote, script: 'abc.sh'
            sshPut remote: remote, from: "Dockerfile_${BUILD_ID}", into: '/root/docker/'
            sshCommand remote: remote, command: "docker build -t testweb:latest -f /root/docker/Dockerfile_${BUILD_ID} ."
	    sshCommand remote: remote, command: "docker tag testweb learndockerwithme/testweb:latest"
	    sshCommand remote: remote, command: "docker tag testweb learndockerwithme/testweb:v1.${BUILD_ID}"
            //sh 'docker tag nginxtest nikhilnidhi/nginxtest:latest'
             //sh 'docker tag nginxtest nikhilnidhi/nginxtest:$BUILD_NUMBER'
            //sshCommand remote: remote, command: 'ansible-playbook docker.yml'
            //sshCommand remote: remote, command: 'export BUILD_NUMBER=${BUILD_NUMBER}'
            //sshCommand remote: remote, command: 'echo ${BUILD_NUMBER}>/tmp/test.txt'
            
            //sshRemove remote: remote, path: 'abc.sh'
            
      stage('Docker Push') {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
         //sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          //sh 'docker push shanem/spring-petclinic:latest'
	sshCommand remote: remote, command: "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
	//sshCommand remote: remote, command: "docker login -u learndockerwithme -p K1reit@123"
	sshCommand remote: remote, command: "docker push learndockerwithme/testweb:latest"
	sshCommand remote: remote, command: "docker push learndockerwithme/testweb:v1.${BUILD_ID}"
                sh 'rm -rf Dockerfile_${BUILD_ID}'
		
	stage('container creation'){
		sshCommand remote: remote, command: "ansible-playbook /root/docker/docker.yml -e image_id=learndockerwithme/testweb:v1.${BUILD_ID} -e container_name=testweb_v1_${BUILD_ID}"

		
		}
		
	
      }            
            }
        }
    }
}

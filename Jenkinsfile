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
            sshCommand remote: remote, command: "docker build -t testweb.v1.${BUILD_ID} -f /root/docker/Dockerfile_${BUILD_ID} ."
            sshCommand remote: remote, command: "docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
	    sshCommand remote: remote, command: "docker push learndockerwithme/testweb.v1.${BUILD_ID}:latest"
            //sshCommand remote: remote, command: 'ansible-playbook docker.yml'
            //sshCommand remote: remote, command: 'export BUILD_NUMBER=${BUILD_NUMBER}'
            //sshCommand remote: remote, command: 'echo ${BUILD_NUMBER}>/tmp/test.txt'
            
            //sshRemove remote: remote, path: 'abc.sh'
            
            stage("creating container"){
                //sshCommand remote: remote, command: 'ansible-playbook /root/docker/docker.yml'
                sh 'rm -rf Dockerfile_${BUILD_ID}'
            
            }
        }
    }
}

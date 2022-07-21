def remote = [:]
remote.name = "worker-1"
remote.host = "172.31.24.178"
remote.allowAnyHosts = true

node { 
    
    
        stage('checkout scm'){
           git branch: 'main', url: 'https://github.com/starx46/kubernatesprojects.git'
           sh 'ls -ltrh' 
        
        }
  
    
    withCredentials([sshUserPrivateKey(credentialsId: 'docker-build', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'userName')]) {
        remote.user = userName
        remote.identityFile = identity
        stage("docker image build") { 
            sh 'echo ${BUILD_NUMBER}'
            sshPut remote: remote, from: 'Dockerfile', into: '/root/docker/'
            //sshCommand remote: remote, command: 'docker build -t testweb /root/docker/.'
            sshCommand remote: remote, command: 'export BUILD_NUMBER=${BUILD_NUMBER}'
            sshCommand remote: remote, command: 'echo ${BUILD_NUMBER}'
            //sshScript remote: remote, script: 'abc.sh'
            //sshRemove remote: remote, path: 'abc.sh'
        }
    }
}

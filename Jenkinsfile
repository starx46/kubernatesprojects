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
        stage("SSH Steps Rocks!") {            
            sshPut remote: remote, from: 'Dockerfile', into: '/root/docker/'
            sshCommand remote: remote, command: 'hostname -i'
            //sshScript remote: remote, script: 'abc.sh'
            //sshRemove remote: remote, path: 'abc.sh'
        }
    }
}

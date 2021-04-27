node{
   stage('SCM Checkout'){
       git(credentialsId: 'git-creds-adi', url: 'https://github.com/adipos/adisite.git', branch: 'main')
   }
   stage('Build Docker Image'){
     sh 'docker build -t adisolutions/adisolutions.com:lts .'
   }
   stage('Push Docker Image'){
     withCredentials([usernamePassword(credentialsId: 'dockerhub-adi', passwordVariable: 'pass', usernameVariable: 'user')]) {
        sh "docker login -u ${user} -p ${pass}"
     }
     sh 'docker push adisolutions/adisolutions.com:lts'
   }
   stage('Run Container on Dev Server'){
     def dockerRun = 'docker run --name test-adi-site --expose 80 -e VIRTUAL_HOST=test.adisolutions.com.co -e LETSENCRYPT_HOST=test.adisolutions.com.co -e LETSENCRYPT_EMAIL=andres.guerrero@adisolutions.com.co --net nginx-ssl_default adisolutions/adisolutions.com:lts'
     sshagent(['ssh-adi']) {
       sh "ssh -o StrictHostKeyChecking=no root@209.145.50.68 ${dockerRun}"
     }
   }
}

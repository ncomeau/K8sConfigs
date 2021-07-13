node {

  stage('Get deployment files') {
      /* Cloning the Repository to jenkins-docker Workspace */
      // git branch: 'jake-testing', url: 'https://github.com/JaBarosin/NodeApp.git'
      checkout scm
  }


  stage('Deploy to mK8s') {
    sshagent(['ubuntu-host']) {
      sh "scp -o StrictHostKeyChecking=no pod-v1.yaml jake@192.168.6.44:/k8s/dev/"
      try{
          sh "ssh jake@192.168.6.44 microk8s kubectl apply -f /k8s/dev/pod-v1.yaml"
      }

      catch(error){
          echo "Welp... those didnt exist yet"
          sh "ssh jake@192.168.6.44 microk8s kubectl create -f /k8s/dev/pod-v1.yaml"
      }
    }

  }

}

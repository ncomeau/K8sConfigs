node {

  stage('Get deployment files') {
      /* Cloning the Repository to jenkins-docker Workspace */
      // git branch: 'jake-testing', url: 'https://github.com/JaBarosin/NodeApp.git'
      checkout scm
  }

  stage('Validate K8s-object') {
  try {
    echo "Validate stage... Starting validate test for deployment.yaml."
    sh '/var/jenkins_home/app/cbctl k8s-object validate -f deployment.yaml -o json > deployment_manifest_validate.json'
    // sh 'python3 /var/jenkins_home/app/cbctl_validate_helper.py ${REPO}_${IMAGE}_validate.json > cbctl_policy_no_violations.txt'

  }

  catch (err) {
    violations = true
    echo "Build detected cbctl violations. Review Cbctl scan results."
    // sh 'python3 /var/jenkins_home/app/cbctl_validate_helper.py ${REPO}_${IMAGE}_validate.json > cbctl_policy_violations.txt'

    }
  }


  stage('Deployment test') {
    sshagent(['ubuntu-host']) {
      sh "scp -o StrictHostKeyChecking=no deployment.yaml jake@192.168.6.44:/k8s/dev/"
      try{
          sh "ssh jake@192.168.6.44 microk8s kubectl apply -f /k8s/dev/deployment.yaml && sleep 5"
      }

      catch(error){
          echo "Welp... those didnt exist yet"
          sh "ssh jake@192.168.6.44 microk8s kubectl create -f /k8s/dev/deployment.yaml && sleep 5"
      }

      stage('Connection Test') {
        sh "curl 192.168.6.44:30333"
        echo "Done testing"
      }

      stage('Cleanup') {
        sh "ssh jake@192.168.6.44 microk8s kubectl delete deployment nodeapp"
        sh "ssh jake@192.168.6.44 microk8s kubectl delete service nodeapp-service"
        sh "ssh jake@192.168.6.44 microk8s kubectl get all"

      }

    }

  }

}

# CB Container Demo - K8s configs, Jenkinsfiles, and more!

This repo has several example branches which can be used by SEs to test different K8s resource configurations in a demo environment.

In order to get started, start by setting up your environment by following the [CBC_Container_CICD_Demo](https://github.com/ncomeau/CBC_Container_CICD_Demo) guide.

## Branches

#### _CBC_Container_Demo_

  * Validate and deploy two different k8s configurations to a microk8s cluster
    * Start with the 'bad' deployment with configurations that violate CB Container Policy rules
    * Finish with the 'good' deployment which is deployed and monitored by CB Container resources

###### Setup for Jenkins

  1. Navigate to your Jenkins Dashboard

  2. Select ```New Item``` on the upper left-hand side

  3. Enter a name for your project (i.e. "Bad-Deployment-Pipeline")--> select ```Pipeline``` --> click 'ok' in the bottom left

  4. Inside of the pipeline configuration page enter the following

Pipeline config | Value
--------------------- | ---------------------
Pipeline Definition: | 'Pipeline script from SCM'
SCM: | 'Git'
Repositories - Reposiroty URL: | ```https://github./com/JaBarosin/K8sConfigs.git```
Credentials: | _none_
Branches to build - Branch specifier: | ```*/CBC_Container_Demo```
Repository Browser: | '(Auto)''
Script Path: | ```bad/Jenkinsfile```

  5. Click 'apply' & 'save

  6. Repeat and create a second Pipeline.

  * Create the second Pipeline with a name like "Good-Deployment-Pipeline"
  * Set the _Script Path_ to ```good/Jenkinsfile```

  7. Select ```Build Now``` for the Job set to the _bad_ job first.

  * View the slack notification which confirms the violations, and provides a breakdown into what specific configurations were the offending rules.

  8. Select ```Build Now``` for the Job set to the _good_ job.

  * View the slack notifications.
  * Curl or navigate to <your-IP-address>:30333 and check to see the container webpage.
  * _Tip_: On your Jenkins host, open a new terminal tab and use the ```watch``` command to monitor microk8s.
    - Enter command: ```watch microk8s kubectl get all -n nodeapp``` (crtl+c = exit)

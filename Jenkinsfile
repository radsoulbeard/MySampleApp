@Library('piper-lib-os') _

node(){
  stage('Prepare')   {
      deleteDir()
      checkout scm
      setupCommonPipelineEnvironment script:this
  }

 // stage('Build')   {
 //     mtaBuild script:this
 // }
  
  stage('Test') {
     dockerExecute ( script: this, dockerImage: 'node:8-stretch' ) {
	   sh '''
		   cd MySampleApp && npm config set @sap:registry "https://npm.sap.com" && npm install && sudo npm run-script test
	   '''
	 }
  }

  stage('Deploy')   {
      cloudFoundryDeploy script:this, deployTool:'mtaDeployPlugin', verbose: true
  }
}

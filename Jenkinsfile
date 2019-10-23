@Library('piper-lib-os') _

node(){
  stage('Prepare')   {
      deleteDir()
      checkout scm
      setupCommonPipelineEnvironment script:this
  }

  stage('Build')   {
      mtaBuild script:this
  }
  
  stage('Test') {
     npmExecute script: this {
	   sh 'cd MySampleApp && npm config set @sap:registry "https://npm.sap.com" && npm install && npm run-script test'
	 }
  }

  stage('Deploy')   {
      cloudFoundryDeploy script:this, deployTool:'mtaDeployPlugin', verbose: true
  }
}

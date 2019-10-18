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
     karmaExecuteTests script: this, runCommand:'npm run-script test'
  }

  stage('Deploy')   {
      cloudFoundryDeploy script:this, deployTool:'mtaDeployPlugin', verbose: true
  }
}

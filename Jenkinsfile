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
     dockerExecute ( script: this, dockerOptions: '--user 0:0' ,dockerImage: 'geekykaran/headless-chrome-node-docker:latest' ) {
	   sh '''
	       echo "Create Test Runner User"
		   useradd -m -u 1000 test_runner
		   echo "Create Test Runner Sudoer Group"
		   groupadd test_runner_sudo
		   usermod -a -G test_runner_sudo test_runner
		   su -c "echo '%test_runner_sudo ALL=(ALL:ALL) NOPASSWD:ALL' >> /etc/sudoers"
		   su -l test_runner
		   cd MySampleApp
		   npm config set @sap:registry "https://npm.sap.com" 
		   npm install 
		   npm run-script test
	   '''
	 }
  }

  stage('Deploy')   {
      cloudFoundryDeploy script:this, deployTool:'mtaDeployPlugin', verbose: true
  }
}

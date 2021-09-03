@Library('piper-lib-os') _

pipeline {
  agent any
  environment {
     IT_IS_A_TEST = 'Test'

  }
  options {
    skipDefaultCheckout()
    timestamps()
  }
  stages {
    
    stage('Build golang') {
      steps {
        script {
          deleteDir()
      git url:'https://github.com/radsoulbeard/jenkins-library.git', branch: 'master'
      dockerExecute(script: this, dockerImage: 'docker.wdf.sap.corp:50000/golang:1.15') {
        sh 'GOOS=linux GOARCH=amd64 CGO_ENABLED=0 go build -o piper . && chmod 777 piper'
      }
      sh "./piper --help"
      stash name: 'piper-bin', includes: 'piper'

        }
      }
      
  }
    
    stage('Init') {
        steps {
          script {
            deleteDir()
            scmInfo = checkout(scm)
            writeFile file: ".pipeline/config.yml",
                      text: configYml()
          
            stash name: 'all'
            sh "ls -la"
            sh "cat .pipeline/config.yml"
          
            piperPipelineStageInit script: this,
                                 skipCheckout : true,
                                 scmInfo      : scmInfo,
                                 stashConten. : ['all'],
                                 configFile: '.pipeline/config.yml'                                 
        }
      
      }
      
   
    }
  }
}

def configYml() {
  """|general:
     |  buildTool: 'mta'
     |  verbose: true
     |""".stripMargin()
}

@Library('piper-lib-os') _

pipeline {
  agent none
  environment {
     IT_IS_A_TEST = 'Test'

  }
  options {
    skipDefaultCheckout()
    timestamps()
  }
  stages {
    stage('Init') {
      node('Initnode'){
        steps {
          script {
            scmInfo = checkout(scm)
            writeFile file: ".pipeline/config.yml",
                      text: configYml()
          
            stash name: 'all'
            sh "ls -la"
            sh "cat .pipeline/config.yml"
          
            piperPipelineStageInit script: this,
                                 skipCheckout : true,
                                 scmInfo      : scmInfo,
                                 configFile: '.pipeline/config.yml'                                 
        }
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
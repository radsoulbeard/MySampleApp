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
    stage('Init') {
      steps {
        script {
          scmInfo = checkout(scm)
          writeFile file: ".pipeline/config.yml",
                    text: configYml()
          
          stash name: 'all' includes: '*/**'
          sh "ls -la"
          sh "cat .pipeline/config.yml"
          
          piperPipelineStageInit script: this,
                                 skipCheckout : true,
                                 scmInfo      : scmInfo

                                 
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
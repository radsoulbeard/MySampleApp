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
          
          piperPipelineStageInit script: this,
                                 skipCheckout : true,
                                 scmInfo      : scmInfo

          sh "ls -la"
                                 
        }
      }
    }
  }
}

def configYml() {
  """|general:
     |  buildTool: 'mta'
     |""".stripMargin()
}
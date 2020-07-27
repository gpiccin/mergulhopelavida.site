@Library('heelal-jenkins-library@master') _
pipeline {
  agent any
  environment {
    ZIP_FILE = "deploy-${env.GIT_COMMIT}-${env.BUILD_NUMBER}.zip"
  }
  stages { 
    stage("Build"){
        steps{
          sh 'find . -type f \\( -name "*.md" -or -name "Jenkinsfile" \\) -delete'
          zip zipFile: "$ZIP_FILE"
        }
    }
    stage('Backup'){
      steps{
        heelalSiteBackup(
            folder: "mergulhopelavida.com.br"
        )
      }
    }
    stage('Publish') {
      steps {
        heelalSitePublish(
            folder: "mergulhopelavida.com.br",
            zipFile: "$ZIP_FILE"
        )
        heelalConfigureProxy(
            folder: "mergulhopelavida.com.br",
            file: "mergulhopelavida.com.br.conf"
        )
      }
    }
    stage('Reload proxy') {
      steps {
        heelalReloadProxy()
      }
    }
  }
}
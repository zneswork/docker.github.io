def dtrVpnAddress = "vpn.corp-us-east-1.aws.dckr.io"
def ucpBundle = [file(credentialsId: "ucp-bundle", variable: 'UCP')]

pipeline {
  agent none
  stages {
    stage( 'docker.github.io' ) {
      agent { label 'ubuntu-1604-aufs-stable' }
      environment {
        VERSION = "test-jenkins"
      }
      stages {
        stage( 'build and push stage image' ) {
          when {
            branch 'jenkins-master'
          }
          steps {
            withCredentials([usernamePassword(credentialsId: 'ally-docker', passwordVariable: 'PWD', usernameVariable: 'USR')]) {
              echo 'would log into docker here'
              echo 'would buld stage image here'
              echo 'would push stage image here'
            }
          }
        }
        stage( 'build and push prod image' ) {
          when {
            branch 'jenkins-published'
          }
          steps {
            withCredentials([usernamePassword(credentialsId: 'ally-docker', passwordVariable: 'PWD', usernameVariable: 'USR')]) {
              echo 'would log into docker here'
              echo 'would buld prod image here'
              echo 'would push prod image here'
            }
          }
        }
        stage( 'update docs-stage' ) {
          when {
            branch 'jenkins-master'
          }
          steps {
            withVpn(dtrVpnAddress) {
              withCredentials(ucpBundle) {
                echo 'would unzip ucp bundle here'
              }
              withCredentials([usernamePassword(credentialsId: 'ally-docker', passwordVariable: 'PWD', usernameVariable: 'USR')]) {
                echo 'would ssh into machine here'
                echo 'would update docs-stage service here'
              }
            }
          }
        }
        stage( 'update docs-prod' ) {
          when {
            branch 'jenkins-published'
          }
          steps {
            withVpn(dtrVpnAddress) {
              withCredentials(ucpBundle) {
                echo 'would unzip ucp bundle here'
              }
              withCredentials([usernamePassword(credentialsId: 'ally-docker', passwordVariable: 'PWD', usernameVariable: 'USR')]) {
                echo 'would ssh into machine here'
                echo 'would update docs-prod service here'
              }
            }
          }
        }
      }
    }
  }
}

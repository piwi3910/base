pipeline {
  environment {
    imagename = "piwi3910/base"
    registryCredential = 'docker_reg'
    alpine_dockerImage = ''
    ubuntu_dockerImage = ''
  }
  agent {
    kubernetes {
      yamlFile 'buildpod.yaml'
    }
  }
  stages {
    stage('Build base Alpine image with dind') {
        steps {
          container('docker') {
            script {
              alpine_dockerImage = docker.build("${env.imagename}:alpine_${BUILD_ID}", "${WORKSPACE}/alpine" ) 
            }
          }
        }    
    }
    stage('Push base Alpine image to DockerHub') {
        steps {
          container('docker') {
            script {
              docker.withRegistry( '', registryCredential ) {
                alpine_dockerImage.push('alpine_latest')
                alpine_dockerImage.push('latest')
              }
            }
          }
        }    
    }  
    stage('Build base Ubuntu image with dind') {
        steps {
          container('docker') {
            script {
              ubuntu_dockerImage = docker.build("${env.imagename}:ubuntu_${BUILD_ID}", "${WORKSPACE}/ubuntu/20.04/" ) 
            }
          }
        }    
    }
    stage('Push base Ubuntu image to DockerHub') {
        steps {
          container('docker') {
            script {
              docker.withRegistry( '', registryCredential ) {
                ubuntu_dockerImage.push('ubuntu_latest')
              }
            }
          }
        }    
    }    
  }
}

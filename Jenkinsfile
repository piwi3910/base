pipeline {
  environment {
    imagename = "piwi3910/base"
    registryCredential = 'docker_reg'
    dockerImage ''
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
              dockerImage = docker.build("${env.imagename}:alpine_latest", "${WORKSPACE}/alpine" ) 
            }
          }
        }    
    }  
    stage('Build base Ubuntu image with dind') {
        steps {
          container('docker') {
            sh 'docker version && DOCKER_BUILDKIT=1 docker build --progress plain -t testing `pwd`/ubuntu/20.04'
          }
        }  
    }    
  }
}

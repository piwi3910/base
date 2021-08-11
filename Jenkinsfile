pipeline {
  agent {
    kubernetes {
      yamlFile 'buildpod.yaml'
    }
  }
  stages {
    stage('Build base Alpine image with dind') {
        steps {
          container('docker') {
            sh 'docker version && DOCKER_BUILDKIT=1 docker build --progress plain -t testing alpine/Dockerfile'
            }
        }    
    }  
    stage('Build base Ubuntu image with dind') {
        steps {
          container('docker') {
            sh 'docker version && DOCKER_BUILDKIT=1 docker build --progress plain -t testing ubuntu/20.04/Dockerfile'
          }
        }  
    }    
  }
}

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
            sh 'ls -lah'
            sh 'docker version && DOCKER_BUILDKIT=1 docker build --progress plain -t testing `pwd`/alpine'
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

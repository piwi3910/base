pipeline {
  environment {
    imagename = "piwi3910/base"
    registryCredential = 'docker_reg'
    alpine_dockerImage = ''
    ubuntu_dockerImage = ''
    alpine_version = "3.13.5"
    ubuntu_version = "20.04"
  }
  agent none
  triggers { cron('0 0 * * 0')} // trigger the master branch to build weekly
  stages {
    stage('Run Builds AMD64') {
        agent {
          kubernetes {
            yamlFile 'buildpod_amd64.yaml'
          }
        }
      parallel {
        stage('Build base Alpine image with dind') {
            steps {
              container('docker_amd64') {
                script {
                  docker.withRegistry( '', registryCredential ) {
                    alpine_dockerImage = docker.build("${env.imagename}:alpine_${BUILD_ID}", "--build-arg VERSION=${alpine_version} ${WORKSPACE}/alpine" ) 
                  }
                }  
              }
            }    
        }
        
        stage('Build base Ubuntu image with dind') {
            steps {
              container('docker_amd64') {
                script {
                  docker.withRegistry( '', registryCredential ) {
                    ubuntu_dockerImage = docker.build("${env.imagename}:ubuntu_${BUILD_ID}", "--build-arg VERSION=${ubuntu_version} ${WORKSPACE}/ubuntu/" ) 
                  }  
                }
              }
            }    
        }
      }
    }
    stage('Run Dockerhub Push AMD64') {
        agent {
          kubernetes {
            yamlFile 'buildpod_amd64.yaml'
          }
        }
      parallel {            
        stage('Push base Alpine image to DockerHub') {
            steps {
              container('docker_amd64') {
                script {
                  docker.withRegistry( '', registryCredential ) {
                    alpine_dockerImage.push('alpine_latest')
                    alpine_dockerImage.push('alpine_${alpine_version}')
                    alpine_dockerImage.push('latest')
                  }
                }
              }
            }    
        }  

        stage('Push base Ubuntu image to DockerHub') {
            steps {
              container('docker_amd64') {
                script {
                  docker.withRegistry( '', registryCredential ) {
                    ubuntu_dockerImage.push('ubuntu_latest')
                    ubuntu_dockerImage.push('ubuntu_${ubuntu_version}')
                  }
                }
              }
            }    
        }
      }  
    }      
  }
}

pipeline {
  environment {
    imagename = "piwi3910/base"
    docker_hub_cred = 'docker_reg'
    nexus_cred = 'f06e38c2-f3b3-43a2-a9d1-3dabb64ca873'
    alpine_dockerImage = ''
    ubuntu_dockerImage = ''
    alpine_version = "3.13.5"
    ubuntu_version = "20.04"
  }
  agent none
  triggers { cron('0 0 * * 0')} // trigger the master branch to build weekly
  stages {
    stage('Run Builds') {
      parallel {
        stage('Build base Alpine image with dind') {
          agent {
            kubernetes {
              yamlFile 'buildpod_arm64.yaml'
            }
          }
            steps {
              container('docker') {
                script {
                  docker.withRegistry( '', nexus_cred ) {                  
                    alpine_dockerImage = docker.build("${env.imagename}:alpine_${BUILD_ID}", "--build-arg VERSION=${alpine_version} ${WORKSPACE}/alpine" ) 
                  }  
                }  
              }
            }    
        }
        
        stage('Build base Ubuntu image with dind') {
          agent {
            kubernetes {
              yamlFile 'buildpod_arm64.yaml'
            }
          }          
            steps {
              container('docker') {
                script {
                  docker.withRegistry( '', nexus_cred ) {
                    ubuntu_dockerImage = docker.build("${env.imagename}:ubuntu_${BUILD_ID}", "--build-arg VERSION=${ubuntu_version} ${WORKSPACE}/ubuntu/" ) 
                  }  
                }
              }
            }    
        }
      }
    }
    stage('Run Dockerhub Push') {
      parallel {            
        stage('Push base Alpine image to DockerHub') {
          agent {
            kubernetes {
              yamlFile 'buildpod_arm64.yaml'
            }
          }          
            steps {
              container('docker') {
                script {
                  docker.withRegistry( '', docker_hub_cred ) {
                    alpine_dockerImage.push('alpine_latest')
                    alpine_dockerImage.push('alpine_${alpine_version}')
                    alpine_dockerImage.push('latest')
                  }
                }
              }
            }    
        }  

        stage('Push base Ubuntu image to DockerHub') {
          agent {
            kubernetes {
              yamlFile 'buildpod_arm64.yaml'
            }
          }          
            steps {
              container('docker') {
                script {
                  docker.withRegistry( '', docker_hub_cred ) {
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

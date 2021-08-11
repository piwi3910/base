pipeline {
<<<<<<< HEAD
  agent {
    kubernetes {
      //cloud 'kubernetes'
      defaultContainer 'kaniko'
      yaml '''
        kind: Pod
        spec:
          containers:
          - name: kaniko
            image: gcr.io/kaniko-project/executor:debug
            imagePullPolicy: Always
            command:
            - sleep
            args:
            - 99d
            volumeMounts:
              - name: jenkins-docker-cfg
                mountPath: /kaniko/.docker
          volumes:
          - name: jenkins-docker-cfg
            projected:
              sources:
              - secret:
                  name: regcred
                  items:
                    - key: .dockerconfigjson
                      path: config.json
'''
    }
  }
  stages {
    stage('Build base Alpine image with Kaniko') {
      steps {
        sh '/kaniko/executor -f `pwd`/alpine/Dockerfile -c `pwd` --insecure --skip-tls-verify --cache=true --destination=piwi3910/base:alpine_latest --destination=piwi3910/base:latest'
      }
    }  
    stage('Build base Ubuntu image with Kaniko') {
      steps {
        sh '/kaniko/executor -f `pwd`/ubuntu/20.04/Dockerfile -c `pwd` --insecure --skip-tls-verify --cache=true --destination=piwi3910/base:ubuntu_latest'
      }
    } 
  }
}
=======
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
    stage('Run Builds') {
      parallel {
        stage('Build base Alpine image with dind') {
            steps {
              container('docker') {
                script {
                  alpine_dockerImage = docker.build("${env.imagename}:alpine_${BUILD_ID}", "${WORKSPACE}/alpine" ) 
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
      }
    }
    stage('Run Dockerhub Push') {
      parallel {            
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
  }
}
>>>>>>> dind

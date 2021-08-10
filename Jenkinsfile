pipeline {
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
      stage('Build base Ubuntu image with Kaniko') {
      steps {
        sh '/kaniko/executor -f `pwd`/ubuntu/20.04/Dockerfile -c `pwd` --insecure --skip-tls-verify --cache=true --destination=piwi3910/base:ubuntu_latest'
      }
    }
  }
}
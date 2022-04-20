pipeline {
  environment {
    PROJECT = "fis-poc-346406"
    IMAGE_TAG = "asia.gcr.io/fis-poc-346406/frontend-baseline"
    SVC_ACCOUNT_KEY = credentials('jenkins-sa')
  }
  agent {
    kubernetes {
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
  name: kaniko
labels:
  component: ci
spec:
  restartPolicy: Never
  containers:
  - name: gcloud
    image: gcr.io/google.com/cloudsdktool/cloud-sdk:latest
    command:
    - cat
    tty: true
  - name: kaniko
    image: gcr.io/kaniko-project/executor:v1.7.0-debug
  volumes:
  - name: google-cloud-key
    secret:
      secretName: jenkins-sa
    volumeMounts:
    - name: google-cloud-key
      mountPath: /var/secrets/google
    env:
    - name: GOOGLE_APPLICATION_CREDENTIALS
      value: /var/secrets/google/key.json
    command:
    - cat
    tty: true
  """
}
  }
  stages {
    stage('test') {
      steps {
        container('gcloud') {
            sh '''
            echo -n $SVC_ACCOUNT_KEY | base64 -d > "${HOME}/serviceaccount.json"
            cat ${HOME}/serviceaccount.json
            gcloud auth activate-service-account --key-file=${HOME}/serviceaccount.json
            gcloud auth list
            '''
        }
      }
      
      }
    stage('Bake') {
      steps {
        container('kaniko') {
            sh '''
            pwd
            /kaniko/executor --dockerfile=./Dockerfile --context=/home/jenkins/agent/workspace/frontend --destination=asia.gcr.io/fis-poc-346406/frontend-baseline --destination=asia.gcr.io/fis-poc-346406/frontend-baseline 
            '''
        }
      }
      
      }
    }
}

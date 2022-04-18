pipeline {
  environment {
    PROJECT = "gj-playground"
    APP_NAME = "hipster-adservice"
    CLUSTER = "test__spinnaker "
    CLUSTER_ZONE = "us-central1-c"
    IMAGE_TAG = "asia.gcr.io/gj-playground/frontend-baseline"
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
  - name: kaniko
    image: gcr.io/kaniko-project/executor:debug
    command:
    - cat
    tty: true
  - name: gcloud
    image: gcr.io/google.com/cloudsdktool/cloud-sdk:latest
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
            gcloud auth list
            gcloud config set account fis-jenkins-sa@fis-poc-346406.iam.gserviceaccount.com 
            '''
        }
      }
    }
    stage('Bake') {
      steps {
        container('kaniko') {
            sh '''
            pwd
            /kaniko/executor --dockerfile=./Dockerfile --context=/home/jenkins/agent/workspace/frontend --destination=asia.gcr.io/gj-playground/frontend-baseline --destination=asia.gcr.io/gj-playground/frontend-baseline 
            '''
        }
      }
      
      }
    }
}

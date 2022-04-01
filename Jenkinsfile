pipeline {
  environment {
    PROJECT = “gj-playground”
    APP_NAME = “hipster-adservice”
    CLUSTER = “test-spinnaker”
    CLUSTER_ZONE = “asia-southeast1-a”
    IMAGE_TAG = “gcr.io/gj-playground/frontend”
    JENKINS_CRED = gj-playground
  }
  agent {
    kubernetes {
      defaultContainer ‘jnlp’
      yaml “”""
apiVersion: v1
kind: Pod
metadata:
labels:
  component: ci
spec:
  # Use service account that can deploy to all namespaces
  containers:
  - name: java
    image: openjdk:8-slim
    command:
    - cat
    tty: true
  - name: gcloud
    image: gcr.io/google.com/cloudsdktool/cloud-sdk:latest
    command:
    - cat
    tty: true
  
  “”""
}
  }
  stages {
    stage(‘Test’) {
      steps {
        container(‘java’) {
          sh “”"
            echo “******** currently executing Test stage ********”
          “”"
        }
      }
    }
    stage('Bake') {
      steps {
        container(name: 'kaniko') {
            sh '''
            /kaniko/executor --dockerfile `pwd`/Dockerfile --context `pwd` --destination=gcr.io/gj-playground/adservice . --destination=gcr.io/gj-playground/adservice .
            '''
        }
      }
    }
}

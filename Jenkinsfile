pipeline {
  environment {
    SVC_ACCOUNT_KEY = credentials('secret')
    PROJECT = "fis-poc-346406"
    APP_NAME = "hipster-adservice"
    CLUSTER = "fis-poc-1"
    CLUSTER_ZONE = "asia-southeast1-a"
    IMAGE_TAG = "asia.gcr.io/fis-poc-346406/frontend-baseline"
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
  
  """
}
  }
  stages {
    stage('Bake') {
      steps {
        container('kaniko') {
            sh '''
            pwd
            echo $SVC_ACCOUNT_KEY
            wget https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-381.0.0-linux-x86_64.tar.gz
            gcloud auth activate-service-account --key-file=$SVC_ACCOUNT_KEY
            /kaniko/executor --dockerfile=./Dockerfile --context=/home/jenkins/agent/workspace/frontend --destination=asia.gcr.io/fis-poc-346406/frontend-baseline --destination=asia.gcr.io/fis-poc-346406/frontend-baseline 
            '''
        }
      }
      
      }
    }
}

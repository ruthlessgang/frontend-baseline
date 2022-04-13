pipeline {
  environment {
    SVC_ACCOUNT_KEY = credentials('sa-jenkins')
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
            curl https://sdk.cloud.google.com | bash
            gcloud auth service-account fis-jenkins-sa@fis-poc-346406.iam.gserviceaccount.com --key-file=./creds/serviceaccount.json --project=fis-poc-346406
            /kaniko/executor --dockerfile=./Dockerfile --context=/home/jenkins/agent/workspace/frontend --destination=asia.gcr.io/fis-poc-346406/frontend-baseline --destination=asia.gcr.io/fis-poc-346406/frontend-baseline 
            '''
        }
      }
      
      }
    }
}

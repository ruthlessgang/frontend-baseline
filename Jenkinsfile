pipeline {
  environment {
    PROJECT = "gj-playground"
    APP_NAME = "hipster-adservice"
    CLUSTER = "test__spinnaker"
    CLUSTER_ZONE = "us-central1-c"
    IMAGE_TAG = "gcr.io/fis-poc-346406/frontend-baseline"
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
            /kaniko/executor --dockerfile=./Dockerfile --context=/home/jenkins/agent/workspace/frontend --destination=gcr.io/fis-poc-346406/frontend-baseline --destination=gcr.io/fis-poc-346406/frontend-baseline 
            '''
        }
      }
      
      }
    }
}

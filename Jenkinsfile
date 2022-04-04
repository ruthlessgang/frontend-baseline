pipeline {
  environment {
    PROJECT = "gj-playground"
    APP_NAME = "hipster-adservice"
    CLUSTER = "test-spinnaker"
    CLUSTER_ZONE = "us-central1-c"
    IMAGE_TAG = "gcr.io/gj-playground/frontend-baseline"
    JENKINS_CRED = "gj-playground"
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
            /kaniko/executor --dockerfile 'pwd'/Dockerfile --context 'pwd' --destination=gcr.io/gj-playground/frontend-baseline --destination=gcr.io/gj-playground/frontend-baseline 
            '''
        }
      }
      
      }
    }
}

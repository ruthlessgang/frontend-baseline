pipeline {
  environment {
    PROJECT = "fis-poc-346406"
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
    image: gcr.io/kaniko-project/executor:debug
    command:
    - /busybox/cat
    tty: true
    volumeMounts:
      - name: jenkins-sa
        mountPath: /secret
    env:
      - name: GOOGLE_APPLICATION_CREDENTIALS
        value: /secret/fis-poc-346406-739f80e0e3b4.json
  volumes:
    - name: jenkins-sa
      secret:
        secretName: jenkins-sa
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
            docker pull ubuntu
            docker tag ubuntu asia.gcr.io/fis-poc-346406/frontend-baseline
            docker push asia.gcr.io/fis-poc-346406/frontend-baseline
            '''
        }
      }
      
      }
    stage('Bake') {
      steps {
        container('kaniko') {
            sh '''
            #!/busybox/sh 
            pwd
            /kaniko/executor --dockerfile=./Dockerfile --context=/home/jenkins/agent/workspace/frontend --destination=asia.gcr.io/fis-poc-346406/frontend-baseline --destination=asia.gcr.io/fis-poc-346406/frontend-baseline    
            '''
        }
      }
      
      }
    }
}

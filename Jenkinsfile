pipeline {
  agent {
    kubernetes {
      label 'sample-app'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
labels:
  component: ci
spec:
  containers:
  - name: gcloud-kubectl-docker
    image: gcr.io/extended-arcana-408715/gcloud-kubectl-docker
    command:
    - cat
    tty: true
"""
}
  }
  stages {
    stage('Build and push image with Container Builder') {
      steps {
        container('gcloud-kubectl-docker') {
          sh "gcloud auth activate-service-account --key-file=extended-arcana-408715-08a0471e7544.json"
          sh "gcloud config set project extended-arcana-408715"
          sh "PYTHONUNBUFFERED=1 gcloud builds submit -t gcr.io/extended-arcana-408715/emailservice ."
        }
      }
   }
    stage('deployment'){
      steps{
        container('gcloud-kubectl-docker'){
          sh"gcloud container clusters get-credentials mycluster --zone asia-east1-a --project extended-arcana-408715"
          sh"kubectl apply -f emailservice.yaml"
}
}

      }
    }
  }

pipeline {
  agent {
    kubernetes {
      label 'agent-s'
    }
  }
  stages {
    stage('Clone') {
      steps {
        git branch: 'main',
          credentialsId: 'github-home-kops-token',
          url: 'https://github.com/home-kops/k8s-vault.git'
      }
    }

    stage('Verify') {
      steps {
        container('jnlp') {
          sh 'kubectl kustomize ./vault --enable-helm'
          sh '''
            helm lint ./unsealer && \
              helm template ./unsealer -f ./unsealer/values.yaml
          '''
        }
      }
    }
  }
}

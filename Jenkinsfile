pipeline {
  agent {
    dockerfile {
      filename 'Dockerfile'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh '''script scripts/build.sh
'''
      }
    }

    stage('Test') {
      steps {
        sh '''scripts/test.sh
'''
      }
    }

  }
}
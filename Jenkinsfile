pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh '''chmod +x scripts/build.sh
scripts/build.sh
'''
      }
    }

    stage('Test') {
      steps {
        sh '''chmod +x scripts/build.sh
scripts/test.sh'''
      }
    }

  }
}
pipeline {
  agent {
    docker {
      image 'node:lts-alpine'
    }

  }
  stages {
    stage('Git checkout') {
      steps {
        sh '${scm}'
      }
    }

    stage('Build') {
      steps {
        sh '''chmod +x scripts/build.sh
scripts/build.sh'''
      }
    }

    stage('Test') {
      steps {
        sh '''chmod +x scripts/test.sh
scripts/test.sh'''
      }
    }

  }
}
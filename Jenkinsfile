pipeline {
  agent any
  stages {
    stage('Git checkout') {
      steps {
        sh '${scm}'
      }
    }

    stage('Build') {
      agent {
        docker {
          image 'node:lts-alpine'
        }

      }
      steps {
        sh '''chmod +x scripts/build.sh
sh scripts/build.sh'''
      }
    }

    stage('Test') {
      agent {
        docker {
          image 'node:lts-alpine'
        }

      }
      steps {
        sh '''npm install react-scripts --save
chmod +x scripts/test.sh
sh scripts/test.sh'''
      }
    }

    stage('Docker build') {
      steps {
        script {
          docker.build("release:${env.BUILD_NUMBER}", ".")
        }

      }
    }

    stage('Docker push') {
      steps {
        script {
          sh "docker images"
          def localImage = "release:${env.BUILD_NUMBER}"
          def repositoryName = "blackmessiah/${localImage}"

          sh "docker tag ${localImage} ${repositoryName}"
          docker.withRegistry('https://registry.hub.docker.com') {
            def image = docker.image("${repositoryName}");
            image.push()
          }
        }

      }
    }

  }
}
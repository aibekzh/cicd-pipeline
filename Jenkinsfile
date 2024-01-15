pipeline {
  agent any
  stages {
    stage('Git checkout') {
      steps {
        sh '${scm}'
      }
    }

    stage('Build') {
      parallel {
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
          steps {
            sh '''chmod +x scripts/test.sh
scripts/test.sh'''
          }
        }

      }
    }

    stage('Test') {
      agent {
        docker {
          image 'node:lts-alpine'
        }

      }
      steps {
        sh '''chmod +x scripts/test.sh
scripts/test.sh'''
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
          def localImage = "release:${env.BUILD_NUMBER}"
          def repositoryName = "blackmessiah/${localImage}"

          sh "docker tag ${localImage} ${repositoryName} "
          docker.withRegistry('https://registry.hub.docker.com') {
            def image = docker.image("${repositoryName}");
            image.push()
          }
        }

      }
    }

  }
}
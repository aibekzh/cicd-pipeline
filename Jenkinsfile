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
sh scripts/build.sh'''
      }
    }

    stage('Test') {
      steps {
        sh '''chmod +x scripts/test.sh
scripts/test.sh'''
      }
    }

    stage('Docker build') {
      agent any
      steps {
        script {
          docker.build("release:${env.BUILD_NUMBER}", ".")
        }

      }
    }

    stage('Docker push') {
      agent any
      steps {
        sh ''' echo "Pushing the image to docker hub"
          def localImage = "release:${env.BUILD_NUMBER}"
          def repositoryName = "blackmessiah/${localImage}"

          // Create a tag that going to push into DockerHub
          sh "docker tag ${localImage} ${repositoryName} "
          docker.withRegistry(\'https://registry.hub.docker.com\') {
            def image = docker.image("${repositoryName}");
            image.push()
          }'''
        script {
          echo "Pushing the image to docker hub"
          def localImage = "release:${env.BUILD_NUMBER}"
          def repositoryName = "blackmessiah/${localImage}"

          // Create a tag that going to push into DockerHub
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
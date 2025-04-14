pipeline {
  agent any

  environment {
    DOCKER_HUB_CREDS = credentials('dockerhub-creds')
  }

  stages {
    stage('Clone Repo') {
      steps {
        checkout scm
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          // Build the image regardless of the branch
          sh './build.sh'
        }
      }
    }

    stage('Push to DockerHub') {
      when {
        anyOf {
          branch 'dev'
          branch 'main'
        }
      }
      steps {
        script {
          // Decide tag and repo based on branch
          def tag = env.BRANCH_NAME == 'main' ? 'prod' : 'dev'
          def imageName = "manishapandey20/devops-app-${tag}:latest"

          // Tag, login, push
          sh "docker tag manishapandey20/devops-app:latest ${imageName}"
          sh "echo ${DOCKER_HUB_CREDS_PSW} | docker login -u ${DOCKER_HUB_CREDS_USR} --password-stdin"
          sh "docker push ${imageName}"
        }
      }
    }
  }
}




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

    stage('React App Already Built') {
      steps {
        echo 'React app is pre-built. Skipping npm steps.'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh './build.sh'
      }
    }

    stage('Push to DockerHub') {
      steps {
        script {
          def tag = env.BRANCH_NAME == 'main' ? 'prod' : 'dev'
          def imageName = "manishapandey20/devops-app-${tag}:latest"

          sh "docker tag manishapandey20/devops-app:latest ${imageName}"
          sh "echo ${DOCKER_HUB_CREDS_PSW} | docker login -u ${DOCKER_HUB_CREDS_USR} --password-stdin"
          sh "docker push ${imageName}"
        }
      }
    }
  }
}



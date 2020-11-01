pipeline {

  agent any

  stages {
    stage('Checkout Source') {
      steps {
        git url:'https://github.com/aymenabdelwahed/hellowhale.git', branch:'master'
      }
    }
    
    stage("Build image") {
      steps {
        script {
          appimage = docker.build("aymenabdelwahed/hellowhale:v0.${env.BUILD_ID}")
        }
      }
    }
    
    stage("Push image") {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            appimage.push("latest")
            appimage.push("${env.BUILD_ID}")
          }
        }
      }
    }
    
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "hellowhale.yml", kubeconfigId: "kubeConfig")
        }
      }
    }
  }
}

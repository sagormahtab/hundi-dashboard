pipeline {
  agent any
  stages {
    stage("Build") {
      steps {
        sh "yarn"
        sh "yarn build"
      }
    }
    stage("Deploy") {
      steps {
        sh "sudo rm -rf /var/www/hundi-demo"
        sh "sudo cp -r ${WORKSPACE}/build /var/www/hundi-demo"
      }
    }
  }
}
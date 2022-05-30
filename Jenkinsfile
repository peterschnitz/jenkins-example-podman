pipeline {
  agent any
  stages {
    stage("verify tooling") {
      steps {
        sh '''
          podman version
          podman info
          podman-compose version 
          curl --version
          jq --version
        '''
      }
    }
    stage('Login to private repos'){
       steps {
        sh 'podman login -u ${rep_user} -p ${rep_pass} repository.tffauto.no'
       }
    } 
    stage('Prune Podman data') {
      steps {
        sh 'podman system prune -a --volumes -f'
      }
    }
    stage('Start container') {
      steps {
        sh 'podman-compose up -d --no-color'
        sh 'podman-compose ps'
      }
    }
    stage('Run tests against the container') {
      steps {
        sh 'curl http://localhost:3000/param?query=demo | jq'
      }
    }
  }
  post {
    always {
      sh 'podman-compose down -v'
      sh 'podman-compose ps'
    }
  }
}

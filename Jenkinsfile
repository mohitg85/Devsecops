pipeline {
  agent any
  stages {
    stage('Pulling Repo') {
      agent any
      steps {
        git(poll: true, url: 'https://github.com/mohitg85/Devsecops.git', changelog: true)
      }
    }

    stage('Test and Build') {
      steps {
        sh 'mvn test install'
      }
    }

    stage('Setup App server ') {
      steps {
        ansiblePlaybook(becomeUser: 'sudo', playbook: '/home/miki/docker.yml', inventory: '/home/miki/host.ini')
      }
    }

    stage('Prod Deployment') {
      steps {
        ansiblePlaybook(playbook: '/home/miki/prodep.yml', inventory: '/home/miki/host.ini')
      }
    }

  }
}
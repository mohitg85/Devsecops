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

    stage('Staging deployment') {
      parallel {
        stage('Setup Staginng ') {
          steps {
            ansiblePlaybook(becomeUser: 'sudo', playbook: '/home/miki/docker.yml', inventory: '/home/miki/host.ini')
          }
        }

        stage('Stage Deployment') {
          steps {
            ansiblePlaybook(playbook: '/home/miki/stagedep.yml', inventory: '/home/miki/host.ini')
          }
        }

      }
    }

    stage('Approval for prod ') {
      steps {
        input(message: 'Do you approve Prod deployment', ok: 'Ok')
      }
    }

    stage('Prod Deployment') {
      steps {
        ansiblePlaybook(playbook: '/home/miki/dockerp.yml', inventory: '/home/miki/host.ini')
        ansiblePlaybook(playbook: '/home/miki/prodep.yml', inventory: ' /home/miki/host.ini')
      }
    }

  }
}
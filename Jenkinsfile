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
            ansiblePlaybook(playbook: '/home/miki/jenkins.yml', inventory: '/home/miki/host.ini')
          }
        }

      }
    }

    stage('Prod Deployment') {
      parallel {
        stage('Setup Prod') {
          steps {
            ansiblePlaybook(playbook: '/home/miki/dockerp.yml', inventory: '/home/miki/host.ini')
          }
        }

        stage('Prod Deployment') {
          steps {
            ansiblePlaybook(playbook: '/home/miki/jenkinsp.yml', inventory: '/home/miki/host.ini')
          }
        }

      }
    }

  }
}
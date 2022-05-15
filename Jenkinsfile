pipeline {
  agent any
  stages {
    stage('Ping') {
      steps {
        sh 'ansible all -i hosts -m ping -f 5'
      }
    }

    stage('Install Docker') {
      steps {
        ansiblePlaybook(playbook: 'dependencies.yml', inventory: 'hosts', colorized: true, becomeUser: 'cliente')
        sh 'ansible all -i hosts -a "sudo systemctl enable docker"'
      }
    }

    stage('Donwload Odoo') {
      steps {
        ansiblePlaybook(playbook: 'imagen.yml', colorized: true, becomeUser: 'cliente', inventory: 'hosts')
      }
    }

    stage('Envio docker-compose') {
      steps {
        sh '''


ansible all -i hosts -m copy -a "src=docker-compose.yml dest=/tmp/docker-compose.yml"'''
      }
    }

    stage('Iniciar Odoo') {
      steps {
        ansiblePlaybook(playbook: 'iniciar_docker.yml', colorized: true, becomeUser: 'cliente', inventory: 'hosts')
      }
    }

  }
}

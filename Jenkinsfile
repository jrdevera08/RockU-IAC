pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps { git url: 'https://github.com/jrdevera08/RockU-IAC.git', branch: 'main' }
    }

    stage('Terraform Init') {
      steps {
        dir('terraform') {
          // If Jenkins runs on Windows and you want WSL, use: sh 'wsl terraform init'
          sh 'terraform init'
        }
      }
    }

    stage('Terraform Apply') {
      steps {
        dir('terraform') {
          sh 'terraform apply -auto-approve'
        }
      }
    }

    stage('Generate Inventory') {
      steps {
        dir('terraform') {
          sh "terraform output -raw ansible_inventory > ../ansible/inventory.ini"
        }
      }
    }

    stage('Run Ansible Playbook') {
      steps {
        dir('ansible') {
          // If Jenkins uses Windows agent + WSL, replace with: sh 'wsl ansible-playbook -i inventory.ini playbook.yml'
          sh 'ansible-playbook -i inventory.ini playbook.yml'
        }
      }
    }
  }

  post {
    success { echo '✅ Deploy pipeline finished.' }
    failure { echo '❌ Deploy pipeline failed.' }
  }
}

pipeline {
  agent {
    node {
      label 'workstation'
    }
  }

  parameters {
    string(name: 'COMPONENT', defaultValue: '', description: 'Which Component')
    string(name: 'ENV', defaultValue: 'prod', description: 'Which Env')
    string(name: 'APP_VERSION', defaultValue: '', description: 'Which Version')

  }

  stages {
    stage('Helm Chart Deploy') {
      steps {
        sh 'aws eks update-kubeconfig --name ${ENV}-eks'
        sh 'helm upgrade -i ${COMPONENT} roboshop'
      }

    }
  }

}

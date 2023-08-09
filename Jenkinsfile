node('workstation') {
  APP_VERSIONS = sh (script: 'aws ecr describe-images --repository-name ${COMPONENT} --query \'imageDetails[*].imageTags\' --output text | sort ', returnStdout:true).trim()
  //APP_VERSIONS = sh (script: 'aws ecr describe-images --repository-name ${COMPONENT} --query \'imageDetails[*].imageTags\' --output text | sort ', returnStdout:true).trim()
}

pipeline {
  agent {
    node {
      label 'workstation'
    }
  }

  parameters {
    string(name: 'COMPONENT', defaultValue: '', description: 'Which Component')
    string(name: 'ENV', defaultValue: 'prod', description: 'Which Env')
    choice(
        name: 'APP_VERSION',
        choices: "${APP_VERSIONS}",
        description: 'to refresh the list, go to configure, disable "this build has parameters", launch build (without parameters)to reload the list and stop it, then launch it again (with parameters)'
    )

  }

  stages {
    stage('Clone App Repo') {
      steps {
        dir('APP') {
          git branch: 'main', url: 'http://github.com/raghudevopsb73/${COMPONENT}'
        }
      }
    }

    stage('Helm Chart Deploy') {
      steps {
        sh 'aws eks update-kubeconfig --name ${ENV}-eks'
        sh 'helm upgrade -i ${COMPONENT} roboshop -f APP/helm.yaml --set image.tag=${APP_VERSION}'
      }

    }
  }

  post {
    always {
      cleanWs()
    }
  }

}

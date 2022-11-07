pipeline {
  agent any

  stages {
    stage('deploy to staging') {
        when { branch "main" }
        steps {
            sh 'echo deploy'
        }
    }
    stage('deploy to staging') {
        when { branch "feature/*" }
        steps {
            sh 'echo psuhed to feture branch'
        }
    }
        stage('deploy to staging') {
        when { branch "release/*" }
        steps {
            sh 'echo pushed to release beanch'
        }
    }

  }

  post {
    always {
      cleanWs()
    }
  }
}
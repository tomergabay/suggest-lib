pipeline {
  agent any

  stages {
    stage ('STAGE 1 calculate and set version') {
      when { branch "release/*" }
      steps {
        sh 'git fetch --tags'

        script {
          majorMinor = env.BRANCH_NAME.split("/")[1]  
          try {
            new_tag = Integer.parseInt(sh(script: "git tag | grep ${majorMinor} | sort --version-sort | tail -1 | cut -d '.' -f 3", returnStdout: true).trim())
            echo "${new_tag}"
            new_tag += 1
          } catch (Exception e) {
            new_tag = 0
          }
          new_version = "${majorMinor}.${new_tag}"
          sh "mvn versions:set -DnewVersion=${newversion}"
        }
      }
    }

    stage ('STAGE 2 build & test') {
      steps {
        sh "java -version"
        sh "which mvn"
        sh "mvn verify"
      }
    }

    stage ('STAGE 3 publish') {
      when {
        anyOf {
            branch "release/*"
            branch "main"
        }
      }
      steps {
        configFileProvider([configFile(fileId: "mvn-settings", variable: "MAVEN_SETTINGS")]) {
            sh 'mvn deploy -s $MAVEN_SETTINGS -DskipTests'
        }
      }
    }

    stage ('STAGE 4 Tag') {
      when { branch "release/*" }
      steps {
        script {
            sh "git clean -f -x"
            sh "git tag -a ${new_version} -m 'version ${new_version}'"
            sh "git push --tag"
        }
      }
    }
  }
}

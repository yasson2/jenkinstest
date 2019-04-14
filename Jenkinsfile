pipeline {
  agent any
  stages {
    stage('Prepare') {
      steps {
        git(url: 'https://github.com/yasson2/jenkinstest.git', branch: 'master', credentialsId: '649a6f77-79a7-44ff-8358-4d12356b0b48')
        fileExists 'script.sh'
        echo 'Prepare complete !'
      }
    }
    stage('Deploy') {
      steps {
        echo 'Deploy ...'
        sh '$WORKSPACE/repo/$BUILD_SCRIPTS/script.sh'
      }
    }
    stage('Test') {
      steps {
        sh '[ -f $FILE ] && echo "$FILE exist"'
      }
    }
    stage('Clean') {
      steps {
        cleanWs(cleanWhenAborted: true, cleanWhenFailure: true, cleanWhenNotBuilt: true, cleanWhenSuccess: true, cleanWhenUnstable: true, deleteDirs: true, disableDeferredWipeout: true, cleanupMatrixParent: true)
      }
    }
  }
  environment {
    BUILD_SCRIPTS_GIT = 'https://github.com/yasson2/jenkinstest.git'
    BUILD_SCRIPTS = 'mypipeline'
    BUILD_HOME = '/var/lib/jenkins/workspace'
    FILE = '/tmp/1.txt'
  }
}
pipeline {
  agent any
  stages {
    stage('Prepare') {
      steps {
        sh '''#install WGET
if ! [ -x "$(command -v wget)" ]; then
  sudo yum-y install wget
  exit 0
fi'''
        sh '''#install MySQL
if ! [ -x "$(command -v mysql)" ]; then
  sudo yum -y install mysql
  exit 0
fi'''
        sh '''#install DOCKER
if ! [ -x "$(command -v docker)" ]; then
  sudo yum install -y docker
  sudo usermod -a -G docker ec2-user
  sudo service docker start
  sudo docker run hello-world
  exit 0
fi'''
      }
    }
    stage('Build') {
      steps {
        sh 'docker stack deploy -c stack.yml wordpress'
        fileExists 'stack.yml'
      }
    }
    stage('Test') {
      steps {
        sh 'curl http://ec2-35-181-91-136.eu-west-3.compute.amazonaws.com'
        sh 'mysql -u exampleuser -pexamplepass'
      }
    }
    stage('Cleaning') {
      steps {
        sh 'docker stack rm wordpress'
        cleanWs(cleanWhenAborted: true, cleanWhenFailure: true, cleanWhenNotBuilt: true, cleanWhenSuccess: true, cleanWhenUnstable: true, cleanupMatrixParent: true, deleteDirs: true, disableDeferredWipeout: true)
      }
    }
  }
}
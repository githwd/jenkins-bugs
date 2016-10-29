#!groovy

@Library('assertions')
import com.markwaite.Assert

/* Only keep the 10 most recent builds. */
properties([[$class: 'BuildDiscarderProperty',
                strategy: [$class: 'LogRotator', numToKeepStr: '10']]])

node("master") {
  stage('Checkout') {
    sh 'echo Working directory is `pwd`'
    sh 'pwd'
    sh 'echo JENKINS_HOME is $JENKINS_HOME' 
  }

  stage('Load a file from GitHub') {
    def helloworld = fileLoader.fromGit('examples/fileLoader/helloworld', 
            'https://github.com/MarkEWaite/jenkins-pipeline-utils.git', 'master', null, '')
    helloworld.printHello()
  }

  stage('Verify') {
    def check = fileLoader.fromGit('src/com/markwaite/Assert', 
            'https://github.com/MarkEWaite/jenkins-bugs.git', 'master', null, '')
    check.logContains(".*Working directory is ${env.JENKINS_HOME}.*", "Working dir report 1 missing")
    check.logContains("Working directory is ${env.JENKINS_HOME}.*", "Working dir report 2 missing")
    check.logContains("${env.JENKINS_HOME}.*", "Working dir report 3 missing")
    check.logContains("JENKINS_HOME is ${env.JENKINS_HOME}.*", "Working dir report 4 missing")
  }
}

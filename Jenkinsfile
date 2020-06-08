pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        build(job: 'build', propagate: true, quietPeriod: 1)
      }
    }

  }
}
pipeline {
  agent any
  stages {
  stage ('test') { // la phase build
  steps {
  bat 'gradle test'
   junit 'build/test-results/test/TEST-Matrix.xml'

     cucumber buildStatus: 'UNSTABLE',
                  reportTitle: 'My report',
                  fileIncludePattern: 'target/report.json',

                  trendsLimit: 10

  }
  }



}
}



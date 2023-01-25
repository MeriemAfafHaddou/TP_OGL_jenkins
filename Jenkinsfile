pipeline {
  agent any
  stages {
  stage("test"){
        steps{
        bat 'gradlew test'
        junit 'build/test-results/test/*.xml'
        cucumber buildStatus: 'UNSTABLE',
        reportTitle: 'My report',
        fileIncludePattern: 'target/report.json'
        }
      }

      stage('SonarQube analysis') {
            steps{
             withSonarQubeEnv("sonar") { // Will pick the global server connection you have configured
               bat 'gradlew sonarqube' }
            }

          }

      stage("Quality Gate") {
                  steps {
                      timeout(time: 1, unit: 'HOURS') {
                          // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                          // true = set pipeline to UNSTABLE, false = don't
                          waitForQualityGate abortPipeline: true
                      }
                  }
              }

       stage("Build") {
                  steps {
                      bat 'gradlew build'
                      bat 'gradlew javadoc'
                      archiveArtifacts 'build/libs/*.jar'
                      archiveArtifacts 'build/docs/'
                  }
              }
       stage("deploy") {
                   steps {
                       bat 'gradlew publish'
                   }
       }
         stage("notify"){
            post{
                 always {
                        echo "End of Pipeline process"
                        mail(subject: 'End of Process Pipeline : Result incoming ...', body: 'End of Process Pipeline : Result incoming ...', from: 'jm_haddou@esi.dz', to: 'ja_abdelaziz@esi.dz')
                      }
                  failure {
                         echo "Deployment failed"
                         mail(subject: 'Deployment failed', body: 'Deployment failed ', from: 'jm_haddou@esi.dz', to: 'ja_abdelaziz@esi.dz')
                       }
                         success {
                               echo "Deployment succeeded"
                               mail(subject: 'Deployment succeeded', body: 'Deployment succeeded ',  from: 'jm_haddou@esi.dz', to: 'ja_abdelaziz@esi.dz')
                               notifyEvents message: 'Bonjour! : <b>Déploiement éffectué !</b> ! ', token: 'wNo2VBaS6pfTx8wP5S8XW0ZS36LivYsb'
                             }
            }
         }

}
}



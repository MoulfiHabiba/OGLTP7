pipeline {
  agent any
  stages {
  stage("test"){
  steps{
      bat './gradlew test'
      archiveArtifacts 'build/test-results/test/'
       cucumber buildStatus: 'UNSTABLE',
                      reportTitle: 'My report',
                      fileIncludePattern: '**/*.json',
                      trendsLimit: 10,
                      classifications: [
                          [
                              'key': 'Browser',
                              'value': 'Firefox'
                          ]
                      ]
}

}
stage("Code Analysis"){
steps{

  withSonarQubeEnv('sonar') {
                    bat "./gradlew sonar"
                }
}
}

 stage("Code Quality") {
            steps {
                waitForQualityGate abortPipeline: true
            }
        }
 stage("Build"){
           steps {
               bat './gradlew build'
               bat './gradlew javadoc'
               archiveArtifacts 'build/libs/*.jar'
           }
           post {
           always {
           echo "Build stage complete" }
           failure {
           echo "Build failed"}
           success {
           echo "Build succeeded" }
           }
           }



stage("Deploy"){
          steps {
              bat './gradlew publish'

          }
          }
stage("notification"){
          steps{
          notifyEvents message: 'New notification', token: 'v1vwv5hma4ribtadfrsz3rbhjii-ba6s'
          mail to: 'kh_moulfi@esi.dz',
               subject: "Succes",
               body:"Najahna"

          }
}

}

}
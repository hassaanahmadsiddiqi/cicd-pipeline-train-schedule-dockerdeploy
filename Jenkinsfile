pipeline {
  agent any
  stages{
    stage('Build'){
      steps{
        echo 'Runing build automation'
        sh './gradlew build --no-daemon'
        archiveArtifacts artifacts: 'dist/trainSchedule.zip'
      }
    }
    stage('Build Docker Image'){
      when {
        branch 'master'
      }
      steps{
        script{
          app = docker.build("hassaansiddiqi20/train-schedule")
          app.inside {
            sh 'echo $(curl localhost:80)'
          }
        }
      }
    }
    stage('Push Docker Image'){
      when {
        branch 'master'
      }
      steps{
        script {
          docker.withRegistry('https://hub.docker.com/u/hassaansiddiqi20','64b261f6-22a6-4832-a9af-ebdf1b25b55b' ){
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
          }
        }
      }
    }
  }
}

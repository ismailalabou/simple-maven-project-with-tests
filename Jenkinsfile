// build triggers on push
pipeline {
  agent {
    kubernetes {
      label BUILD_TAG
      containerTemplate {
        name 'maven'
        image 'maven'
        command 'sleep'
        args 'infinity'
      }
    }
  }
  stages {
    stage('Run') {
      steps {
        container('maven') {
          sh 'mvn -B -ntp -Dmaven.test.failure.ignore verify'
        }
      }
    }
  }
  post {
    success {
      steps {
        junit '**/target/surefire-reports/TEST-*.xml'
      }
      always{
        emailext body: "$currentBuild.currentResult: Job $env.JOB_NAME build $env.BUILD_NUMBER \n More info at : $env.BUILD URL",
        recipientProviders:[developers(),requestor()],
        subject: "Jenkins Build $currentBuild.currentResult: Job $env.JOB_NAME,",
          to "ismail.alabou@uit.ac.ma"
      }
    }
  }
}

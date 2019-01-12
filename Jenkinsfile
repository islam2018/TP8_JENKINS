pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        bat 'gradle build'
      }
    }
    stage('Mail Notification') {
      steps {
        mail(to: 'fm_bouayache@esi.dz', subject: 'Notification Build', body: 'Build réussi')
      }
    }
    stage('Code Analysis') {
      parallel {
        stage('Code Analysis') {
          steps {
            waitForQualityGate true
          }
        }
        stage('Test Reporting') {
          steps {
            jacoco()
          }
        }
      }
    }
    stage('Deployement') {
      steps {
        sh 'gradle uploadArchives'
      }
    }
    stage('Slack Notification') {
      steps {
        slackSend(baseUrl: 'https://jenkinssiege.slack.com/messages/CFC8X1FFV/', channel: '#tpjenkins')
      }
    }
  }
}
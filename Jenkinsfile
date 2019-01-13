pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        bat 'gradle build'
        bat 'gradle javadoc'
        bat 'gradle jar'
      }
    }
    stage('Mail Notification') {
      steps {
        mail(to: 'fm_bouayache@esi.dz', subject: 'Notification Build', body: 'Build reuussi')
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
        bat 'gradle uploadArchives'
      }
    }
    stage('Slack Notification') {
      steps {
        slackSend(baseUrl: 'https://jenkinssiege.slack.com/messages/CFC8X1FFV/', channel: '#tpjenkins')
      }
    }
  }
}
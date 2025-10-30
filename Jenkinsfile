pipeline {
  agent any

  tools {
    jdk 'jdk17'
    maven 'maven3'
  }

  options {
    timestamps()
  }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Build & Test') {
      steps {
        sh 'mvn -v'
        sh 'mvn -B -DskipTests=false test'
      }
      post {
        always {
          junit allowEmptyResults: false, testResults: 'target/surefire-reports/*.xml'
        }
      }
    }
  }

  post {
    success { echo '✅ Build OK : tests au vert.' }
    failure { echo '❌ Échec : check logs & JUnit.' }
    always  { echo "Pipeline terminé sur ${env.JOB_NAME} #${env.BUILD_NUMBER}" }
  }
}


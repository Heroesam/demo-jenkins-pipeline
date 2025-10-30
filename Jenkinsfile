pipeline {
  agent {
    docker {
      image 'maven:3.9.9-eclipse-temurin-17'   // Image Maven + JDK 17
      args '-v $HOME/.m2:/root/.m2'            // Cache Maven entre builds (optionnel)
    }
  }

  options {
    timestamps()
    ansiColor('xterm')
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build & Test') {
      steps {
        sh 'mvn -B -e -DskipTests=false test'
      }
      post {
        always {
          // Publie les rapports JUnit (Surefire) dans Jenkins
          junit allowEmptyResults: false, testResults: 'target/surefire-reports/*.xml'
        }
      }
    }
  }

  post {
    success {
      echo '✅ Build OK : tests au vert.'
    }
    failure {
      echo '❌ Échec : regarde les logs et les rapports JUnit.'
    }
    always {
      echo "Pipeline terminé sur ${env.JOB_NAME} #${env.BUILD_NUMBER}"
    }
  }
}

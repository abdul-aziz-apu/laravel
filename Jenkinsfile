// Add a trivial comment

pipeline {
  agent { docker { image 'composer:2' } }
  environment { COMPOSER_HOME = '/tmp/composer' }
  parameters { booleanParam(name: 'RUN_TESTS', defaultValue: true) }
  stages {
    stage('Install') {
      steps { sh 'composer install --no-interaction --prefer-dist' }
    }
    stage('Test') {
      steps {
        sh 'cp .env.example .env && php artisan key:generate'
        sh 'php artisan test --log-junit junit.xml'
      }
    }
    stage('Secret check') {
      steps {
        withCredentials([string(credentialsId: 'demo-token', variable: 'TOKEN')]) {
          sh 'echo "token length: ${#TOKEN}"'
        }
      }
    }
  }
  post {
    always  { junit 'junit.xml' }
    success { echo 'Green' }
    failure { echo 'Red' }
  }
}


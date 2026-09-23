// Add a trivial comment

pipeline {
  agent { docker { image 'composer:2' } }
  environment { COMPOSER_HOME = '/tmp/composer' }
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
  }
  post {
    always  { junit 'junit.xml' }
    success { echo 'Green' }
    failure { echo 'Red' }
  }
}
pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo "Building the repository"
      }
    }

    stage('Test') {
      steps {
        echo "Running tests..."
        sh 'python3 test_app.py'
      }
    }

    stage('Deploy') {
      steps {
        script {
          // Pauses the pipeline for manual input
          try {
            input message: "Do you want to deploy the application?", ok: "Deploy"
            echo "Deploying the application"
          } catch (Exception e) {
            echo "Deployment was skipped or failed due to input error."
            currentBuild.result = 'ABORTED'  // Mark the build as aborted if input fails
            throw e  // Rethrow to terminate the pipeline gracefully
          }
        }
      }
    }
  }

  post {
    always {
      echo 'The pipeline has completed'
      junit allowEmptyResults: true, testResults: '**/test_reports/*.xml'
    }
    success {
      sh '''
        nohup python3 app.py > log.txt 2>&1 &
      '''
      echo "Flask application is up and running!"
    }
    failure {
      echo 'The pipeline failed during execution'
    }
    aborted {
      echo 'The deployment process was aborted.'
    }
  }
}

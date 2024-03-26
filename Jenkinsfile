pipeline {
    agent {
      node {
        label "linux && java17"
      }
    }
    stages {
        stage("Hello") {
            steps {
                echo ("Hello Pipeline")
            }
        }
    }

    post {
      always {
        echo "I will always say Hello again!"
      }
      success {
        echo "OK Success"
      }
      failure {
        echo "Oh nooo, failire"
      }
      cleanup {
        echo "Don't care success or error"
      }
    }

}
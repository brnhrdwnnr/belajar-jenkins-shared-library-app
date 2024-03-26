pipeline {
    agent {
      node {
        label "linux && java17"
      }
    }
    stages {
        stage("Build") {
            steps {
                script {
                  for (int i = 0; i < 10; i++ ) {
                    echo ("hello script ${i}")
                  }
                }
                echo ("Start Build")
                sh("./mvnw clean compile test-compile")
                echo ("Finish Build")
            }
        }

        stage("Test") {
            steps {
                script {
                  def data = [
                    "firstName": "Eko",
                    "lastName": "Khannedy"
                  ]
                  writeJSON(file: "data.json", json: data)
                }

                echo ("Start Test")
                sh("./mvnw test")
                echo ("Finish Test")
            }
        }

        stage("Deploy") {
            steps {
                echo ("Hello Deploy 1")
                sleep(5)
                echo ("Hello Deploy 2")
                echo ("Hello Deploy 3")
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
        echo "Oh nooo, failure"
      }
      cleanup {
        echo "Don't care success or error"
      }
    }

}
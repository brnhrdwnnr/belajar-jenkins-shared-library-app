pipeline {
    agent none
    environment {
      AUTHOR = "BERNHARD WINNER"
      EMAIL = "bernhardwinner@gmail.com"
      WEB = "https://www.belumada.com"
    }

    // triggers {
    //   cron("*/5 * * * *")
    // }

    parameters {
      string(name: "NAME", defaultValue: "Guest", description: "What is your name")
      text(name: "DESCRIPTION", defaultValue: "", description: "Tell me about yourself")
      booleanParam(name: "DEPLOY", defaultValue: false, description: "Need to deploy")
      choice(name: "SOCIAL_MEDIA", choices: ['INSTAGRAM', 'FACEBOOK', 'TWITTER'], description: "Which social media")
      password(name: "SECRET", defaultValue: "", description: "Encrypt Key")
    }

    options {
      disableConcurrentBuilds()
      timeout(time: 10, unit: 'MINUTES')
    }
    
    stages {

       stage("OS Setup") {
          matrix {
            axes {
              axis {
                name "OS"
                values "linux", "windows", "mac"
              }
              axis {
                name "ARCH"
                values "32", "64"
              }
            }
            stages {
              stage("OS Setup") {
                agent {
                  node {
                    label "linux && java17"
                  }
                }
                steps {
                  echo ("Setup ${OS} ${ARC}")
                }
              }
            }

          }
       }

       stage("Preparation") {
            parallel {
              stage("Prepare Java") {
                agent {
                  node {
                    label "linux && java17"
                  }
                }
                steps {
                  echo("Prepare Java")
                  sleep(5)
                }
                
              }
              stage("Prepare Maven") {
                agent {
                  node {
                    label "linux && java17"
                  }
                }
                steps {
                  echo("Prepare Maven")
                  sleep(5)
                }
              }

            }
       }
        stage("Parameter") {
            agent {
              node {
                label "linux && java17"
              }
            }
            steps {
              echo("Hello: ${params.NAME}")
              echo("Your description is: ${params.DESCRIPTION}")
              echo("Your social media is: ${params.SOCIAL_MEDIA}")
              echo("Need to deploy: ${params.DEPLOY} to deploy!")
              echo("You secret is : ${params.SECRET}")
            }
        }


        stage("Prepare") {
            environment {
              APP = credentials("bernhard_rahasia")
            }
            agent {
              node {
                label "linux && java17"
              }
            }
            steps {
              echo("App User : ${APP_USR}")
              echo("App Password : ${APP_PSW}")
              sh('echo "App Password: ${APP_PSW}" > "rahasia.txt"')
              echo("Author : ${AUTHOR}")
              echo("Email : ${EMAIL}")
              echo("Web : ${WEB}")
              echo("Start Job : ${env.JOB_NAME}")
              echo("Start Build : ${env.BUILD_NUMBER}")
              echo("Branch Name : ${env.BRANCH_NAME}")
            }
        }


        stage("Build") {
            agent {
              node {
                label "linux && java17"
              }
            }
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
            agent {
              node {
                label "linux && java17"
              }
            }
            steps {
                script {
                  def data = [
                    "firstName": "Eko",
                    "lastName": "Khannedy"
                  ]
                  writeJSON file: "data.json", json: data
                }

                echo ("Start Test")
                sh("./mvnw test")
                echo ("Finish Test")
            }
        }

        stage("Deploy") {
            input {
                message "Can we deploy?"
                ok "Yes of course"
                submitter "bernhard, winner"
                parameters {
                  choice(name: "TARGET_ENV", choices: ['DEV', 'QA', 'PROD'], description: "Which Environment?")
                }
            }
            agent {
                node {
                 label "linux && java17"
                }
            }
            steps {
                echo ("Deploy to ${TARGET_ENV}")
            }
        }

        stage("Release") {
            when {
              expression {
                return params.DEPLOY
              }
            }
            agent {
                node {
                 label "linux && java17"
                }
            }
            steps {
                echo ("Release it")
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
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    app = docker.build("rstocker373/train-schedule")
                    app.inside {
                        sh 'echo $(curl localhost 80:80)'
                    }
                }
            }
        }
        stage('Push Docker Image") {
              when {
                  branch 'master'
              }
              steps {
                  Script {
                      docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                          app.push("${env.BUILD_NUMBER}")
                          app.push("latest")
                      }
                  }
              }
              }
              }
              }

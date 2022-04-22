node {
  agent any
  stages {
    stage('build') {
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      steps {
        echo 'compile maven app'
        sh 'mvn compile'
      }
    }

    stage('test') {
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      steps {
        echo 'test maven app'
        sh 'mvn clean test'
      }
    }

      stage('package') {
        agent {
          docker {
            image 'maven:3.6.3-jdk-11-slim'
          }

        }
        steps {
          if (env.BRANCH_NAME == 'master') {
              echo 'package maven app'
              sh 'mvn package -DskipTests'
              archiveArtifacts '**/*.war'
          } else {
              echo 'I execute elsewhere'
          }
        }
    }

      stage('Docker BnP') {
        agent any
        steps {
          if (env.BRANCH_NAME == 'master') {
              script {
                docker.withRegistry('https://index.docker.io/v1/', 'dockerlogin') {
                def dockerImage = docker.build("aayushjain5/sysfoo:v${env.BUILD_ID}", "./")
                dockerImage.push()
                dockerImage.push("latest")
                dockerImage.push("dev")
              }
            }
          } else {
              echo 'I execute elsewhere'
          }
        }
    }

  }
  tools {
    maven 'Maven 3.6.3'
  }
}

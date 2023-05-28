pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    CI = true
    ARTIFACTORY_ACCESS_TOKEN = credentials('artifactory-access-token')
  }
  stages {
    stage('Build') {
      steps {
        sh './mvnw clean install'
      }
    }
    stage('Show Files') {
        environment {
          MY_FILES = sh(script: 'cd mydir && ls -l', returnStdout: true)
        }
        steps {
          sh '''
            echo "$MY_FILES"
          '''
        }
    }
   stage('Upload to Artifactory') {
          
        steps {
          sh '''
            echo "$url"
          '''
        }
    }  
      agent {
        docker {
          image 'releases-docker.jfrog.io/jfrog/jfrog-cli-v2:2.2.0' 
          reuseNode true
        }
      }
      steps {
        sh 'jfrog rt upload --url http://192.168.29.131:8082/artifactory/ --access-token ${ARTIFACTORY_ACCESS_TOKEN} target/demo-0.0.1-SNAPSHOT.jar java-web-app/'
      }
    }
  }
}

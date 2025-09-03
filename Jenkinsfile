pipeline{
  agent any

  tools{
    maven "M3"
    jdk "JDK17"
  }
  environment{
    DOCKERHUB_CREDENTIALS = credentials('dockerCredentials')    
  }
  stages{
    stage('Git Clone'){
      steps{
        git url: 'https://github.com/jkw01065528005/spring-petclinic', branch: 'main'
      }
    }
    stage('Maven Build'){
      steps{
        sh 'mvn -Dmaven.test.failure.ignore=true clean package'
      }
    }
    stage('Docker Image Create') {
      steps{
        sh """
        docker build -t jkwon01/spring-petclinic:$BUILD_NUMBER .
        docker tag jkwon01/spring-petclinic:$BUILD_NUMBER jkwon01/spring-petclinic:latest
        """
      }
    }
    stage('Docker Hub Login'){
      steps{
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stages('Docker Image Push'){
      steps {
        sh 'docker push jkwon01/spring-petclinic:latest'
      }
    }
  }
}

pipeline {
  agent any
  environment {
    IMAGE = 'yourdockerhubuser/java-spring-app:v1'
  }
  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/yourusername/java-spring-app.git'
      }
    }
    stage('Build') {
      steps {
        sh 'mvn clean package -DskipTests'
      }
    }
    stage('Docker Build') {
      steps {
        sh 'docker build -t $IMAGE .'
      }
    }
    stage('Docker Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
          sh 'echo $PASS | docker login -u $USER --password-stdin'
          sh 'docker push $IMAGE'
        }
      }
    }
    stage('Deploy') {
      steps {
        sh '''
          docker stop springapp || true
          docker rm springapp || true
          docker run -d -p 8080:8080 --name springapp $IMAGE
        '''
      }
    }
  }
}
